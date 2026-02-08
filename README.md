<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Онлайн покер для 2 игроков (Telegram)</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            -webkit-tap-highlight-color: transparent;
            -webkit-touch-callout: none;
            user-select: none;
        }
        
        body {
            background: linear-gradient(135deg, #0d4a1e, #0a3618);
            color: #fff;
            min-height: 100vh;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            padding: 10px;
            overflow-x: hidden;
            touch-action: manipulation;
        }
        
        .container {
            max-width: 100%;
            background-color: rgba(0, 0, 0, 0.85);
            border-radius: 20px;
            padding: 15px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.5);
            margin: 0 auto;
        }
        
        h1 {
            text-align: center;
            margin-bottom: 15px;
            color: #FFD700;
            text-shadow: 0 2px 5px rgba(0, 0, 0, 0.5);
            font-size: 1.8rem;
            padding: 0 10px;
        }
        
        .connection-panel {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .connection-status {
            display: inline-block;
            padding: 8px 15px;
            border-radius: 20px;
            font-weight: bold;
            margin-bottom: 15px;
            font-size: 1rem;
        }
        
        .connected {
            background-color: rgba(76, 175, 80, 0.3);
            color: #4CAF50;
            border: 2px solid #4CAF50;
        }
        
        .disconnected {
            background-color: rgba(244, 67, 54, 0.3);
            color: #F44336;
            border: 2px solid #F44336;
        }
        
        .waiting {
            background-color: rgba(255, 193, 7, 0.3);
            color: #FFC107;
            border: 2px solid #FFC107;
        }
        
        .connection-buttons {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 15px;
        }
        
        .connection-btn {
            padding: 14px;
            font-size: 1rem;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            touch-action: manipulation;
        }
        
        .create-btn {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            color: white;
        }
        
        .join-btn {
            background: linear-gradient(135deg, #4CAF50, #388E3C);
            color: white;
        }
        
        .invite-section {
            background-color: rgba(0, 0, 0, 0.5);
            padding: 15px;
            border-radius: 12px;
            margin-top: 15px;
            display: none;
        }
        
        .invite-link {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 12px;
            border-radius: 10px;
            margin: 10px 0;
            word-break: break-all;
            font-family: monospace;
            font-size: 0.9rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .copy-btn {
            background: #FFD700;
            color: #000;
            border: none;
            padding: 8px 15px;
            border-radius: 8px;
            font-weight: bold;
            cursor: pointer;
            margin-left: 10px;
            white-space: nowrap;
        }
        
        .game-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            background-color: rgba(255, 255, 255, 0.1);
            padding: 12px;
            border-radius: 12px;
            margin-bottom: 15px;
            font-size: 0.9rem;
        }
        
        .info-item {
            display: flex;
            flex-direction: column;
            align-items: center;
            text-align: center;
        }
        
        .info-label {
            font-size: 0.8rem;
            opacity: 0.8;
            margin-bottom: 3px;
        }
        
        .info-value {
            color: #FFD700;
            font-weight: bold;
            font-size: 1.1rem;
        }
        
        .stage-indicator {
            background-color: rgba(33, 150, 243, 0.2);
            padding: 8px 15px;
            border-radius: 20px;
            text-align: center;
            margin-bottom: 15px;
            font-weight: bold;
            border: 2px solid #2196F3;
            font-size: 1rem;
        }
        
        /* Карточки игроков */
        .player-area {
            background: linear-gradient(135deg, rgba(255, 255, 255, 0.08), rgba(255, 255, 255, 0.03));
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 15px;
            border: 2px solid transparent;
            transition: all 0.3s;
        }
        
        .player-area.active {
            border-color: #FFD700;
            box-shadow: 0 0 15px rgba(255, 215, 0, 0.4);
        }
        
        .player-area.winner {
            border-color: #4CAF50;
            box-shadow: 0 0 15px rgba(76, 175, 80, 0.5);
        }
        
        .player-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .player-name {
            font-size: 1.2rem;
            font-weight: bold;
            color: #FFD700;
        }
        
        .player-bet {
            background-color: rgba(0, 0, 0, 0.5);
            padding: 4px 10px;
            border-radius: 15px;
            font-size: 0.9rem;
        }
        
        .player-cards {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-bottom: 10px;
            min-height: 95px;
        }
        
        .player-status {
            text-align: center;
            font-size: 0.9rem;
            opacity: 0.9;
            min-height: 20px;
        }
        
        /* Игровой стол */
        .table-area {
            background: linear-gradient(135deg, #2d5a2d, #1e3e1e);
            border-radius: 20px;
            padding: 20px 15px;
            margin: 15px 0;
            text-align: center;
            border: 4px solid #8B4513;
            position: relative;
            box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.5);
        }
        
        .pot-display {
            font-size: 1.5rem;
            color: #FFD700;
            margin-bottom: 15px;
            font-weight: bold;
            text-shadow: 0 2px 3px rgba(0, 0, 0, 0.5);
        }
        
        .community-cards {
            display: flex;
            justify-content: center;
            gap: 8px;
            flex-wrap: wrap;
        }
        
        /* Карты */
        .card {
            width: 60px;
            height: 85px;
            background-color: white;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 6px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
            position: relative;
            flex-shrink: 0;
            touch-action: none;
        }
        
        .card.red {
            color: #d50000;
        }
        
        .card.black {
            color: #000000;
        }
        
        .card-back {
            background: linear-gradient(135deg, #1a237e, #283593);
            color: white;
            justify-content: center;
            align-items: center;
            font-size: 1.8rem;
        }
        
        .card-top, .card-bottom {
            font-size: 0.9rem;
            font-weight: bold;
            line-height: 1;
        }
        
        .card-center {
            font-size: 1.8rem;
            text-align: center;
            line-height: 1;
            flex-grow: 1;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .card-bottom {
            transform: rotate(180deg);
        }
        
        /* Элементы управления - Поменяны местами и добавлено повышение ставок */
        .controls {
            background-color: rgba(255, 255, 255, 0.08);
            border-radius: 15px;
            padding: 15px;
            margin-top: 15px;
        }
        
        .bet-controls {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .bet-btn {
            padding: 14px 5px;
            font-size: 0.9rem;
            background: linear-gradient(135deg, #FF9800, #F57C00);
            color: white;
            border: none;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            text-align: center;
            touch-action: manipulation;
        }
        
        .bet-btn:active {
            transform: scale(0.95);
        }
        
        .action-buttons {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-bottom: 15px;
        }
        
        .action-btn {
            padding: 18px 10px;
            font-size: 1.1rem;
            font-weight: bold;
            border: none;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 60px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            touch-action: manipulation;
            position: relative;
            overflow: hidden;
        }
        
        .action-btn:active {
            transform: scale(0.95);
        }
        
        .action-btn:disabled {
            opacity: 0.5;
            transform: none;
            cursor: not-allowed;
        }
        
        /* Кнопки в новом порядке: Фолд, Колл, Проверка, Поднять */
        .fold-btn {
            background: linear-gradient(135deg, #F44336, #D32F2F);
            color: white;
        }
        
        .call-btn {
            background: linear-gradient(135deg, #4CAF50, #388E3C);
            color: white;
        }
        
        .check-btn {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            color: white;
        }
        
        .raise-btn {
            background: linear-gradient(135deg, #9C27B0, #7B1FA2);
            color: white;
        }
        
        .start-btn {
            background: linear-gradient(135deg, #FF9800, #F57C00);
            color: white;
            grid-column: span 2;
        }
        
        .next-round-btn {
            background: linear-gradient(135deg, #607D8B, #455A64);
            color: white;
            grid-column: span 2;
        }
        
        /* Лог игры */
        .game-log {
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 12px;
            padding: 12px;
            margin-top: 15px;
            max-height: 150px;
            overflow-y: auto;
            font-size: 0.85rem;
            line-height: 1.4;
            -webkit-overflow-scrolling: touch;
        }
        
        .log-entry {
            margin-bottom: 6px;
            padding-bottom: 6px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        /* Уведомления */
        .notification {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.9);
            color: white;
            padding: 15px 20px;
            border-radius: 10px;
            z-index: 1000;
            display: none;
            border: 2px solid #FFD700;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.5);
            max-width: 90%;
            text-align: center;
        }
        
        /* Адаптация */
        @media (max-width: 380px) {
            .card {
                width: 52px;
                height: 74px;
            }
            
            .bet-controls {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .action-btn {
                padding: 16px 8px;
                font-size: 1rem;
            }
        }
        
        /* Анимации */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .pulse {
            animation: pulse 0.5s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.3s ease-out;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>♠️ Онлайн Покер для 2 игроков ♥️</h1>
        
        <!-- Панель подключения -->
        <div class="connection-panel">
            <div class="connection-status disconnected" id="connection-status">
                Отключен
            </div>
            <div>Пригласите друга через Telegram</div>
            
            <div class="connection-buttons">
                <button class="connection-btn create-btn" id="create-btn">
                    Создать игру
                </button>
                <button class="connection-btn join-btn" id="join-btn">
                    Присоединиться
                </button>
            </div>
            
            <div class="invite-section" id="invite-section">
                <div>Отправьте эту ссылку другу:</div>
                <div class="invite-link">
                    <span id="invite-link">Загрузка...</span>
                    <button class="copy-btn" id="copy-btn">Копировать</button>
                </div>
                <div style="font-size: 0.8rem; opacity: 0.8;">
                    Скопируйте и отправьте в Telegram
                </div>
            </div>
            
            <div class="invite-section" id="join-section">
                <div>Введите ID игры:</div>
                <input type="text" id="game-id-input" placeholder="ID игры" 
                       style="width:100%; padding:12px; margin:10px 0; border-radius:8px; border:none; font-size:1rem;">
                <button class="connection-btn join-btn" id="join-game-btn" style="width:100%;">
                    Присоединиться к игре
                </button>
            </div>
        </div>
        
        <!-- Игровая информация -->
        <div class="game-info">
            <div class="info-item">
                <span class="info-label">Ваш баланс</span>
                <span id="player-balance" class="info-value">1000</span>
            </div>
            <div class="info-item">
                <span class="info-label">Соперник</span>
                <span id="opponent-balance" class="info-value">1000</span>
            </div>
            <div class="info-item">
                <span class="info-label">Раунд</span>
                <span id="round" class="info-value">1</span>
            </div>
            <div class="info-item">
                <span class="info-label">Текущая ставка</span>
                <span id="current-bet" class="info-value">10</span>
            </div>
        </div>
        
        <div class="stage-indicator" id="stage">Ожидание игроков...</div>
        
        <!-- Соперник -->
        <div class="player-area" id="opponent-area">
            <div class="player-header">
                <div class="player-name" id="opponent-name">Ожидание соперника...</div>
                <div class="player-bet">Ставка: <span id="opponent-bet">0</span></div>
            </div>
            <div class="player-cards" id="opponent-cards"></div>
            <div class="player-status" id="opponent-status">Не подключен</div>
        </div>
        
        <!-- Игровой стол -->
        <div class="table-area">
            <div class="pot-display">Банк: <span id="pot">0</span></div>
            <div class="community-cards" id="community-cards"></div>
        </div>
        
        <!-- Игрок -->
        <div class="player-area" id="player-area">
            <div class="player-header">
                <div class="player-name" id="player-name">Вы</div>
                <div class="player-bet">Ставка: <span id="player-bet">0</span></div>
            </div>
            <div class="player-cards" id="player-cards"></div>
            <div class="player-status" id="player-status">Ожидание...</div>
        </div>
        
        <!-- Управление с новым порядком кнопок -->
        <div class="controls" id="game-controls" style="display:none;">
            <!-- Быстрые ставки -->
            <div class="bet-controls">
                <button class="bet-btn" data-bet="10">+10</button>
                <button class="bet-btn" data-bet="25">+25</button>
                <button class="bet-btn" data-bet="50">+50</button>
                <button class="bet-btn" data-bet="100">+100</button>
            </div>
            
            <!-- Основные действия (новый порядок) -->
            <div class="action-buttons">
                <button class="action-btn fold-btn" id="fold-btn" disabled>Фолд</button>
                <button class="action-btn call-btn" id="call-btn" disabled>Колл</button>
                <button class="action-btn check-btn" id="check-btn" disabled>Проверка</button>
                <button class="action-btn raise-btn" id="raise-btn" disabled>Поднять</button>
                <button class="action-btn start-btn" id="start-btn">Начать игру</button>
                <button class="action-btn next-round-btn" id="next-round-btn" disabled>След. раунд</button>
            </div>
            
            <div class="game-log" id="game-log">
                <div class="log-entry">Пригласите друга для игры!</div>
            </div>
        </div>
    </div>
    
    <!-- Уведомление -->
    <div class="notification" id="notification"></div>

    <script>
        // Игровое состояние
        const gameState = {
            player: { 
                id: null,
                name: 'Игрок 1',
                balance: 1000, 
                bet: 0, 
                hand: [], 
                folded: false, 
                isActive: false 
            },
            opponent: { 
                id: null,
                name: 'Игрок 2',
                balance: 1000, 
                bet: 0, 
                hand: [], 
                folded: false, 
                isActive: false 
            },
            deck: [],
            communityCards: [],
            pot: 0,
            round: 1,
            stage: 'waiting',
            currentBet: 10,
            smallBlind: 10,
            dealer: 'player',
            gameActive: false,
            gameId: null,
            isHost: false,
            connection: null
        };
        
        // WebSocket соединение (симуляция для демо)
        let ws = null;
        
        // DOM элементы
        const connectionStatus = document.getElementById('connection-status');
        const createBtn = document.getElementById('create-btn');
        const joinBtn = document.getElementById('join-btn');
        const inviteSection = document.getElementById('invite-section');
        const joinSection = document.getElementById('join-section');
        const gameIdInput = document.getElementById('game-id-input');
        const joinGameBtn = document.getElementById('join-game-btn');
        const inviteLink = document.getElementById('invite-link');
        const copyBtn = document.getElementById('copy-btn');
        const notification = document.getElementById('notification');
        const gameControls = document.getElementById('game-controls');
        
        // Игровые элементы
        const playerBalanceEl = document.getElementById('player-balance');
        const opponentBalanceEl = document.getElementById('opponent-balance');
        const roundEl = document.getElementById('round');
        const currentBetEl = document.getElementById('current-bet');
        const stageEl = document.getElementById('stage');
        const playerBetEl = document.getElementById('player-bet');
        const playerCardsEl = document.getElementById('player-cards');
        const playerStatusEl = document.getElementById('player-status');
        const playerArea = document.getElementById('player-area');
        const playerNameEl = document.getElementById('player-name');
        const opponentBetEl = document.getElementById('opponent-bet');
        const opponentCardsEl = document.getElementById('opponent-cards');
        const opponentStatusEl = document.getElementById('opponent-status');
        const opponentArea = document.getElementById('opponent-area');
        const opponentNameEl = document.getElementById('opponent-name');
        const potEl = document.getElementById('pot');
        const communityCardsEl = document.getElementById('community-cards');
        const gameLog = document.getElementById('game-log');
        const betBtns = document.querySelectorAll('.bet-btn');
        const checkBtn = document.getElementById('check-btn');
        const callBtn = document.getElementById('call-btn');
        const raiseBtn = document.getElementById('raise-btn');
        const foldBtn = document.getElementById('fold-btn');
        const startBtn = document.getElementById('start-btn');
        const nextRoundBtn = document.getElementById('next-round-btn');
        
        // Масти и значения
        const suits = ['♠️', '♥️', '♦️', '♣️'];
        const values = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        
        // Инициализация
        function init() {
            updateUI();
            createDeck();
            setupEventListeners();
            generatePlayerName();
            addLog("Готов к онлайн игре!");
            showNotification("Создайте игру или присоединитесь");
        }
        
        // Настройка обработчиков событий
        function setupEventListeners() {
            // Кнопки подключения
            createBtn.addEventListener('click', createGame);
            joinBtn.addEventListener('click', () => {
                joinSection.style.display = 'block';
                inviteSection.style.display = 'none';
            });
            
            joinGameBtn.addEventListener('click', joinGame);
            copyBtn.addEventListener('click', copyInviteLink);
            
            // Кнопки ставок
            betBtns.forEach(btn => {
                btn.addEventListener('click', function() {
                    const betAmount = parseInt(this.dataset.bet);
                    increaseBet(betAmount);
                });
            });
            
            // Игровые кнопки
            checkBtn.addEventListener('click', playerCheck);
            callBtn.addEventListener('click', playerCall);
            raiseBtn.addEventListener('click', playerRaise);
            foldBtn.addEventListener('click', playerFold);
            startBtn.addEventListener('click', startGame);
            nextRoundBtn.addEventListener('click', nextRound);
            
            // Обработчики для мобильных устройств
            [checkBtn, callBtn, raiseBtn, foldBtn, startBtn, nextRoundBtn].forEach(btn => {
                btn.addEventListener('touchstart', function(e) {
                    e.preventDefault();
                    if (!this.disabled) this.click();
                });
            });
        }
        
        // Создание игры
        function createGame() {
            gameState.gameId = generateGameId();
            gameState.isHost = true;
            
            // В реальном приложении здесь было бы подключение к WebSocket серверу
            simulateConnection();
            
            // Показываем ссылку для приглашения
            const gameLink = `t.me/share/url?url=https://poker-game.com/join/${gameState.gameId}&text=Присоединяйся к покеру!`;
            inviteLink.textContent = `ID: ${gameState.gameId}`;
            inviteSection.style.display = 'block';
            joinSection.style.display = 'none';
            
            updateConnectionStatus('waiting');
            addLog(`Игра создана! ID: ${gameState.gameId}`);
            showNotification("Отправьте ID игры другу");
        }
        
        // Присоединение к игре
        function joinGame() {
            const gameId = gameIdInput.value.trim();
            if (!gameId) {
                showNotification("Введите ID игры!");
                return;
            }
            
            gameState.gameId = gameId;
            gameState.isHost = false;
            
            // В реальном приложении здесь было бы подключение к WebSocket серверу
            simulateConnection();
            
            updateConnectionStatus('waiting');
            addLog(`Присоединение к игре ${gameId}...`);
            showNotification("Подключение к игре...");
            
            // Симуляция успешного подключения через 1.5 секунды
            setTimeout(() => {
                gameState.opponent.name = 'Хост';
                gameState.player.name = 'Гость';
                playerNameEl.textContent = gameState.player.name;
                opponentNameEl.textContent = gameState.opponent.name;
                
                addLog("Подключено к игре!");
                showNotification("Вы подключены к игре!");
                updateConnectionStatus('connected');
                gameControls.style.display = 'block';
                
                // Симуляция получения уведомления от хоста
                setTimeout(() => {
                    addLog("Хост готов начать игру!");
                }, 500);
            }, 1500);
        }
        
        // Симуляция подключения (для демо)
        function simulateConnection() {
            updateConnectionStatus('connecting');
            
            setTimeout(() => {
                if (gameState.isHost) {
                    updateConnectionStatus('waiting');
                    addLog("Ожидаем второго игрока...");
                } else {
                    updateConnectionStatus('connected');
                    addLog("Подключено!");
                }
                
                gameControls.style.display = 'block';
                startBtn.disabled = false;
            }, 1000);
        }
        
        // Обновление статуса подключения
        function updateConnectionStatus(status) {
            connectionStatus.textContent = {
                'disconnected': 'Отключен',
                'connecting': 'Подключение...',
                'waiting': 'Ожидание игрока...',
                'connected': 'Подключено'
            }[status];
            
            connectionStatus.className = 'connection-status ' + status;
        }
        
        // Копирование ссылки
        function copyInviteLink() {
            navigator.clipboard.writeText(gameState.gameId)
                .then(() => {
                    showNotification("ID скопирован! Отправьте в Telegram");
                })
                .catch(err => {
                    console.error('Ошибка копирования:', err);
                });
        }
        
        // Показать уведомление
        function showNotification(message) {
            notification.textContent = message;
            notification.style.display = 'block';
            notification.classList.add('fade-in');
            
            setTimeout(() => {
                notification.classList.remove('fade-in');
                setTimeout(() => {
                    notification.style.display = 'none';
                }, 300);
            }, 3000);
        }
        
        // Генерация ID игры
        function generateGameId() {
            return Math.random().toString(36).substring(2, 8).toUpperCase();
        }
        
        // Генерация имени игрока
        function generatePlayerName() {
            const names = ['Игрок', 'Покерист', 'Чемпион', 'Мастер', 'Профи', 'Эксперт'];
            const randomName = names[Math.floor(Math.random() * names.length)];
            const randomNum = Math.floor(Math.random() * 100);
            gameState.player.name = `${randomName} ${randomNum}`;
            playerNameEl.textContent = gameState.player.name;
        }
        
        // Увеличить ставку
        function increaseBet(amount) {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            const totalBet = gameState.player.bet + amount;
            
            if (totalBet > gameState.player.balance) {
                showNotification("Недостаточно средств!");
                return;
            }
            
            if (totalBet <= gameState.currentBet) {
                showNotification("Ставка должна быть выше текущей!");
                return;
            }
            
            makeBet(gameState.player, amount);
            addLog(`Вы повышаете ставку на ${amount}`);
            gameState.currentBet = totalBet;
            
            // Симуляция отправки действия оппоненту
            sendActionToOpponent('raise', amount);
            endPlayerTurn();
        }
        
        // Создание колоды
        function createDeck() {
            gameState.deck = [];
            for (let suit of suits) {
                for (let value of values) {
                    gameState.deck.push({
                        suit,
                        value,
                        numericValue: values.indexOf(value) + 2,
                        color: suit === '♥️' || suit === '♦️' ? 'red' : 'black'
                    });
                }
            }
        }
        
        // Перемешивание колоды
        function shuffleDeck() {
            for (let i = gameState.deck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [gameState.deck[i], gameState.deck[j]] = [gameState.deck[j], gameState.deck[i]];
            }
        }
        
        // Начало игры
        function startGame() {
            if (!gameState.connection || gameState.gameActive) return;
            
            resetRound();
            gameState.gameActive = true;
            shuffleDeck();
            dealCards();
            setBlinds();
            updateUI();
            enablePlayerActions();
            
            // Анимация
            playerArea.classList.add('pulse');
            setTimeout(() => playerArea.classList.remove('pulse'), 500);
            
            if (gameState.dealer === 'player') {
                gameState.opponent.isActive = true;
                opponentArea.classList.add('active');
                addLog("Ход соперника...");
                
                // Симуляция хода оппонента
                setTimeout(() => {
                    simulateOpponentAction();
                }, 2000);
            } else {
                gameState.player.isActive = true;
                playerArea.classList.add('active');
                addLog("Ваш ход!");
            }
            
            startBtn.disabled = true;
            addLog(`Раунд ${gameState.round} начался!`);
        }
        
        // Следующий раунд
        function nextRound() {
            gameState.round++;
            startGame();
            nextRoundBtn.disabled = true;
        }
        
        // Раздача карт
        function dealCards() {
            gameState.player.hand = [];
            gameState.opponent.hand = [];
            gameState.communityCards = [];
            
            for (let i = 0; i < 2; i++) {
                gameState.player.hand.push(gameState.deck.pop());
                gameState.opponent.hand.push(gameState.deck.pop());
            }
            
            renderCards();
            
            // В реальном приложении карты оппонента были бы скрыты
            // Для демо мы показываем рубашкой вверх
            opponentCardsEl.innerHTML = '';
            for (let i = 0; i < 2; i++) {
                const cardElement = createCardElement(null, true);
                opponentCardsEl.appendChild(cardElement);
            }
        }
        
        // Установка блайндов
        function setBlinds() {
            const smallBlind = gameState.smallBlind;
            const bigBlind = smallBlind * 2;
            
            if (gameState.dealer === 'player') {
                makeBet(gameState.opponent, bigBlind);
                makeBet(gameState.player, smallBlind);
                addLog(`Соперник: большой блайнд (${bigBlind})`);
                addLog(`Вы: малый блайнд (${smallBlind})`);
            } else {
                makeBet(gameState.player, bigBlind);
                makeBet(gameState.opponent, smallBlind);
                addLog(`Вы: большой блайнд (${bigBlind})`);
                addLog(`Соперник: малый блайнд (${smallBlind})`);
            }
            
            gameState.currentBet = bigBlind;
        }
        
        // Сделать ставку
        function makeBet(player, amount) {
            const bet = Math.min(amount, player.balance);
            player.balance -= bet;
            player.bet += bet;
            gameState.pot += bet;
            if (bet > gameState.currentBet) gameState.currentBet = bet;
        }
        
        // Действия игрока
        function playerCheck() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            if (gameState.currentBet === 0 || gameState.player.bet === gameState.currentBet) {
                addLog("Вы проверяете");
                sendActionToOpponent('check');
                endPlayerTurn();
            } else {
                showNotification("Нельзя проверять");
            }
        }
        
        function playerCall() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            const callAmount = gameState.currentBet - gameState.player.bet;
            if (callAmount <= 0) {
                playerCheck();
                return;
            }
            
            if (callAmount > gameState.player.balance) {
                showNotification("Недостаточно средств!");
                return;
            }
            
            makeBet(gameState.player, callAmount);
            addLog(`Вы делаете колл (${callAmount})`);
            sendActionToOpponent('call', callAmount);
            endPlayerTurn();
        }
        
        function playerRaise() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            // Для демо используем фиксированную ставку
            const raiseAmount = 50;
            const totalBet = gameState.player.bet + raiseAmount;
            
            if (totalBet <= gameState.currentBet) {
                showNotification("Слишком маленькая ставка!");
                return;
            }
            
            if (raiseAmount > gameState.player.balance) {
                showNotification("Недостаточно средств!");
                return;
            }
            
            makeBet(gameState.player, raiseAmount);
            addLog(`Вы повышаете на ${raiseAmount}`);
            gameState.currentBet = totalBet;
            sendActionToOpponent('raise', raiseAmount);
            endPlayerTurn();
        }
        
        function playerFold() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            gameState.player.folded = true;
            addLog("Вы сбрасываете карты");
            sendActionToOpponent('fold');
            endGame();
        }
        
        // Отправка действия оппоненту (симуляция)
        function sendActionToOpponent(action, amount = 0) {
            // В реальном приложении здесь был бы WebSocket.send
            console.log(`Отправка оппоненту: ${action}, ${amount}`);
            
            // Симуляция получения ответа через 1-2 секунды
            if (action !== 'fold') {
                setTimeout(() => {
                    simulateOpponentResponse(action, amount);
                }, 1000 + Math.random() * 1000);
            }
        }
        
        // Симуляция ответа оппонента
        function simulateOpponentResponse(playerAction, playerAmount) {
            if (gameState.opponent.folded || !gameState.gameActive) return;
            
            const actions = ['call', 'raise', 'fold'];
            const randomAction = actions[Math.floor(Math.random() * actions.length)];
            
            switch (randomAction) {
                case 'call':
                    const callAmount = gameState.currentBet - gameState.opponent.bet;
                    if (callAmount > 0 && callAmount <= gameState.opponent.balance) {
                        makeBet(gameState.opponent, callAmount);
                        addLog(`Соперник делает колл (${callAmount})`);
                    } else {
                        addLog("Соперник проверяет");
                    }
                    break;
                    
                case 'raise':
                    const raiseAmount = Math.min(50, gameState.opponent.balance);
                    const totalBet = gameState.opponent.bet + raiseAmount;
                    if (totalBet > gameState.currentBet && raiseAmount <= gameState.opponent.balance) {
                        makeBet(gameState.opponent, raiseAmount);
                        gameState.currentBet = totalBet;
                        addLog(`Соперник повышает на ${raiseAmount}`);
                    }
                    break;
                    
                case 'fold':
                    gameState.opponent.folded = true;
                    addLog("Соперник сбрасывает карты");
                    endGame();
                    return;
            }
            
            // Переход к следующей стадии или обратно к игроку
            if (checkForRoundEnd()) return;
            
            gameState.player.isActive = true;
            playerArea.classList.add('active');
            updateUI();
        }
        
        // Симуляция действия оппонента
        function simulateOpponentAction() {
            if (!gameState.opponent.isActive || gameState.opponent.folded) return;
            
            const actions = ['check', 'call', 'raise', 'fold'];
            const randomAction = actions[Math.floor(Math.random() * actions.length)];
            
            switch (randomAction) {
                case 'check':
                    addLog("Соперник проверяет");
                    break;
                    
                case 'call':
                    const callAmount = gameState.currentBet - gameState.opponent.bet;
                    if (callAmount > 0 && callAmount <= gameState.opponent.balance) {
                        makeBet(gameState.opponent, callAmount);
                        addLog(`Соперник делает колл (${callAmount})`);
                    }
                    break;
                    
                case 'raise':
                    const raiseAmount = Math.min(100, gameState.opponent.balance);
                    const totalBet = gameState.opponent.bet + raiseAmount;
                    if (totalBet > gameState.currentBet && raiseAmount <= gameState.opponent.balance) {
                        makeBet(gameState.opponent, raiseAmount);
                        gameState.currentBet = totalBet;
                        addLog(`Соперник повышает на ${raiseAmount}`);
                    }
                    break;
                    
                case 'fold':
                    gameState.opponent.folded = true;
                    addLog("Соперник сбрасывает карты");
                    endGame();
                    return;
            }
            
            gameState.opponent.isActive = false;
            opponentArea.classList.remove('active');
            
            if (checkForRoundEnd()) return;
            
            gameState.player.isActive = true;
            playerArea.classList.add('active');
            updateUI();
        }
        
        // Завершение хода игрока
        function endPlayerTurn() {
            gameState.player.isActive = false;
            playerArea.classList.remove('active');
            
            if (checkForRoundEnd()) return;
            
            gameState.opponent.isActive = true;
            opponentArea.classList.add('active');
            
            // Симуляция хода оппонента
            setTimeout(() => {
                simulateOpponentAction();
            }, 1500);
        }
        
        // Проверка окончания раунда
        function checkForRoundEnd() {
            if (gameState.player.folded || gameState.opponent.folded) {
                endGame();
                return true;
            }
            
            if (gameState.player.bet === gameState.opponent.bet && gameState.player.bet >= gameState.currentBet) {
                advanceStage();
                return true;
            }
            
            return false;
        }
        
        // Следующая стадия
        function advanceStage() {
            switch (gameState.stage) {
                case 'preflop':
                    gameState.stage = 'flop';
                    dealCommunityCards(3);
                    addLog("Флоп: 3 общие карты");
                    break;
                case 'flop':
                    gameState.stage = 'turn';
                    dealCommunityCards(1);
                    addLog("Терн: 4-я карта");
                    break;
                case 'turn':
                    gameState.stage = 'river';
                    dealCommunityCards(1);
                    addLog("Ривер: 5-я карта");
                    break;
                case 'river':
                    gameState.stage = 'showdown';
                    endGame();
                    return;
            }
            
            gameState.player.bet = 0;
            gameState.opponent.bet = 0;
            gameState.currentBet = 0;
            
            // Чередуем первого ходящего
            if (Math.random() > 0.5) {
                gameState.player.isActive = true;
                playerArea.classList.add('active');
                addLog("Ваш ход!");
            } else {
                gameState.opponent.isActive = true;
                opponentArea.classList.add('active');
                addLog("Ход соперника...");
                
                setTimeout(() => {
                    simulateOpponentAction();
                }, 1500);
            }
            
            updateUI();
        }
        
        // Выдача общих карт
        function dealCommunityCards(count) {
            for (let i = 0; i < count; i++) {
                if (gameState.deck.length > 0) {
                    gameState.communityCards.push(gameState.deck.pop());
                }
            }
            renderCards();
        }
        
        // Окончание игры
        function endGame() {
            gameState.gameActive = false;
            gameState.player.isActive = false;
            gameState.opponent.isActive = false;
            playerArea.classList.remove('active');
            opponentArea.classList.remove('active');
            
            // Определение победителя
            let winner = determineWinner();
            
            // Выплата банка
            if (winner === 'player') {
                gameState.player.balance += gameState.pot;
                addLog(`🎉 Вы выиграли ${gameState.pot} монет!`);
                playerArea.classList.add('winner');
                
                // Анимация выигрыша
                playerArea.classList.add('pulse');
                setTimeout(() => playerArea.classList.remove('pulse'), 1000);
            } else if (winner === 'opponent') {
                gameState.opponent.balance += gameState.pot;
                addLog(`😢 Соперник выиграл ${gameState.pot} монет`);
                opponentArea.classList.add('winner');
            } else if (winner === 'split') {
                const halfPot = Math.floor(gameState.pot / 2);
                gameState.player.balance += halfPot;
                gameState.opponent.balance += halfPot;
                addLog(`🤝 Ничья! Банк разделен`);
            }
            
            // Показ карт оппонента
            showOpponentCards();
            
            // Обновление UI
            updateUI();
            
            // Включение кнопки следующего раунда
            nextRoundBtn.disabled = false;
            
            // Смена дилера
            gameState.dealer = gameState.dealer === 'player' ? 'opponent' : 'player';
            
            addLog("Раунд завершен");
        }
        
        // Определение победителя (упрощенная версия)
        function determineWinner() {
            if (gameState.player.folded) return 'opponent';
            if (gameState.opponent.folded) return 'player';
            
            // Упрощенная оценка для демо
            const playerScore = evaluateSimpleHand(gameState.player.hand, gameState.communityCards);
            const opponentScore = evaluateSimpleHand(gameState.opponent.hand, gameState.communityCards);
            
            if (playerScore > opponentScore) return 'player';
            if (opponentScore > playerScore) return 'opponent';
            return 'split';
        }
        
        // Упрощенная оценка руки
        function evaluateSimpleHand(playerHand, communityCards) {
            const allCards = [...playerHand, ...communityCards];
            
            // Проверяем пары и тройки
            const values = {};
            allCards.forEach(card => {
                values[card.value] = (values[card.value] || 0) + 1;
            });
            
            let score = 0;
            let pairs = 0;
            
            // Находим комбинации
            for (const value in values) {
                if (values[value] === 2) {
                    pairs++;
                    score += 2;
                } else if (values[value] === 3) {
                    score += 4;
                } else if (values[value] === 4) {
                    score += 8;
                }
            }
            
            if (pairs === 2) score += 1;
            if (pairs === 1 && score === 4) score += 2;
            
            const sortedCards = allCards.sort((a, b) => b.numericValue - a.numericValue);
            score += sortedCards[0].numericValue * 0.01;
            
            return score;
        }
        
        // Показ карт оппонента
        function showOpponentCards() {
            opponentCardsEl.innerHTML = '';
            gameState.opponent.hand.forEach(card => {
                const cardElement = createCardElement(card);
                opponentCardsEl.appendChild(cardElement);
            });
        }
        
        // Создание элемента карты
        function createCardElement(card, isBack = false) {
            const cardElement = document.createElement('div');
            
            if (isBack) {
                cardElement.className = 'card card-back';
                cardElement.innerHTML = '🂠';
            } else {
                cardElement.className = `card ${card.color}`;
                cardElement.innerHTML = `
                    <div class="card-top">${card.value}<br>${card.suit}</div>
                    <div class="card-center">${card.suit}</div>
                    <div class="card-bottom">${card.value}<br>${card.suit}</div>
                `;
            }
            
            return cardElement;
        }
        
        // Отрисовка всех карт
        function renderCards() {
            // Карты игрока
            playerCardsEl.innerHTML = '';
            gameState.player.hand.forEach(card => {
                const cardElement = createCardElement(card);
                playerCardsEl.appendChild(cardElement);
            });
            
            // Общие карты
            communityCardsEl.innerHTML = '';
            gameState.communityCards.forEach(card => {
                const cardElement = createCardElement(card);
                communityCardsEl.appendChild(cardElement);
            });
        }
        
        // Сброс раунда
        function resetRound() {
            gameState.player.bet = 0;
            gameState.opponent.bet = 0;
            gameState.pot = 0;
            gameState.player.folded = false;
            gameState.opponent.folded = false;
            gameState.player.isActive = false;
            gameState.opponent.isActive = false;
            gameState.stage = 'preflop';
            gameState.currentBet = 0;
            
            playerArea.classList.remove('winner', 'active');
            opponentArea.classList.remove('winner', 'active');
            
            createDeck();
        }
        
        // Включение действий игрока
        function enablePlayerActions() {
            checkBtn.disabled = false;
            callBtn.disabled = false;
            raiseBtn.disabled = false;
            foldBtn.disabled = false;
            
            // Обновляем кнопки быстрых ставок
            betBtns.forEach(btn => {
                btn.disabled = false;
            });
        }
        
        // Отключение действий игрока
        function disablePlayerActions() {
            checkBtn.disabled = true;
            callBtn.disabled = true;
            raiseBtn.disabled = true;
            foldBtn.disabled = true;
            
            betBtns.forEach(btn => {
                btn.disabled = true;
            });
        }
        
        // Добавление записи в лог
        function addLog(message) {
            const timestamp = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            const logEntry = document.createElement('div');
            logEntry.className = 'log-entry';
            logEntry.textContent = `[${timestamp}] ${message}`;
            
            gameLog.appendChild(logEntry);
            
            setTimeout(() => {
                gameLog.scrollTop = gameLog.scrollHeight;
            }, 100);
            
            if (gameLog.children.length > 20) {
                gameLog.removeChild(gameLog.firstChild);
            }
        }
        
        // Обновление UI
        function updateUI() {
            playerBalanceEl.textContent = gameState.player.balance;
            opponentBalanceEl.textContent = gameState.opponent.balance;
            playerBetEl.textContent = gameState.player.bet;
            opponentBetEl.textContent = gameState.opponent.bet;
            potEl.textContent = gameState.pot;
            roundEl.textContent = gameState.round;
            currentBetEl.textContent = gameState.currentBet;
            
            // Отображаем стадию игры
            const stageNames = {
                'waiting': 'Ожидание игроков...',
                'preflop': 'Pre-flop',
                'flop': 'Flop',
                'turn': 'Turn',
                'river': 'River',
                'showdown': 'Showdown'
            };
            stageEl.textContent = stageNames[gameState.stage] || gameState.stage;
            
            // Обновляем статусы
            playerStatusEl.textContent = gameState.player.isActive ? 'Ваш ход' : 
                                       gameState.player.folded ? 'Сбросил карты' : 'Ожидание';
            opponentStatusEl.textContent = gameState.opponent.isActive ? 'Ходит...' : 
                                         gameState.opponent.folded ? 'Сбросил карты' : 'Ожидание';
            
            // Обновляем кнопку колла
            const callAmount = gameState.currentBet - gameState.player.bet;
            callBtn.textContent = callAmount > 0 ? `Колл (${callAmount})` : 'Колл';
            
            // Обновляем кнопку проверки
            const canCheck = gameState.currentBet === 0 || gameState.player.bet === gameState.currentBet;
            checkBtn.textContent = canCheck ? 'Проверка' : 'Пропуск';
        }
        
        // Запуск игры при загрузке
        document.addEventListener('DOMContentLoaded', init);
        
        // Предотвращаем масштабирование на двойной тап
        let lastTap = 0;
        document.addEventListener('touchend', function(event) {
            const currentTime = new Date().getTime();
            const tapLength = currentTime - lastTap;
            if (tapLength < 500 && tapLength > 0) {
                event.preventDefault();
            }
            lastTap = currentTime;
        }, {passive: false});
        
        // Отключаем контекстное меню на долгий тап
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
        }, false);
    </script>
</body>
</html>
