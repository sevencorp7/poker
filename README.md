<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Мобильный покер с ботом</title>
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
        
        /* Элементы управления */
        .controls {
            background-color: rgba(255, 255, 255, 0.08);
            border-radius: 15px;
            padding: 15px;
            margin-top: 15px;
        }
        
        .bet-control {
            margin-bottom: 15px;
            padding: 12px;
            background-color: rgba(0, 0, 0, 0.3);
            border-radius: 12px;
        }
        
        .bet-slider-container {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-top: 8px;
            position: relative;
        }
        
        .bet-slider {
            flex: 1;
            height: 40px; /* Увеличиваем высоту для удобного касания */
            -webkit-appearance: none;
            appearance: none;
            background: linear-gradient(to right, #4CAF50, #FFC107, #F44336);
            outline: none;
            border-radius: 20px;
            cursor: pointer;
            position: relative;
            z-index: 1;
        }
        
        /* Стили для мобильного слайдера */
        .bet-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background: #FFD700;
            border: 3px solid #fff;
            cursor: pointer;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            position: relative;
            z-index: 2;
        }
        
        .bet-slider::-moz-range-thumb {
            width: 44px;
            height: 44px;
            border-radius: 50%;
            background: #FFD700;
            border: 3px solid #fff;
            cursor: pointer;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
            position: relative;
            z-index: 2;
        }
        
        .bet-amount {
            font-size: 1.2rem;
            font-weight: bold;
            color: #FFD700;
            min-width: 60px;
            text-align: center;
            padding: 8px 12px;
            background: rgba(0, 0, 0, 0.5);
            border-radius: 10px;
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
            -webkit-user-select: none;
            user-select: none;
            position: relative;
            overflow: hidden;
        }
        
        .action-btn:active {
            transform: scale(0.95);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        
        .action-btn:disabled {
            opacity: 0.5;
            transform: none;
            cursor: not-allowed;
            box-shadow: none;
        }
        
        /* Эффект нажатия для мобильных */
        .action-btn::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 5px;
            height: 5px;
            background: rgba(255, 255, 255, 0.5);
            opacity: 0;
            border-radius: 100%;
            transform: scale(1, 1) translate(-50%, -50%);
            transform-origin: 50% 50%;
        }
        
        .action-btn:active::after {
            animation: ripple 1s ease-out;
        }
        
        @keyframes ripple {
            0% {
                transform: scale(0, 0);
                opacity: 0.5;
            }
            20% {
                transform: scale(25, 25);
                opacity: 0.3;
            }
            100% {
                transform: scale(40, 40);
                opacity: 0;
            }
        }
        
        .check-btn {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            color: white;
        }
        
        .call-btn {
            background: linear-gradient(135deg, #4CAF50, #388E3C);
            color: white;
        }
        
        .raise-btn {
            background: linear-gradient(135deg, #FF9800, #F57C00);
            color: white;
        }
        
        .fold-btn {
            background: linear-gradient(135deg, #F44336, #D32F2F);
            color: white;
        }
        
        .start-btn {
            background: linear-gradient(135deg, #9C27B0, #7B1FA2);
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
        
        /* Помощь */
        .help-toggle {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 60px;
            height: 60px;
            background: linear-gradient(135deg, #FFD700, #FFC107);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            font-weight: bold;
            color: #000;
            z-index: 100;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            cursor: pointer;
            border: 3px solid white;
            touch-action: manipulation;
        }
        
        .help-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.95);
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            display: none;
            -webkit-overflow-scrolling: touch;
        }
        
        .help-content {
            background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
            border-radius: 20px;
            padding: 25px;
            max-width: 500px;
            width: 90%;
            max-height: 85vh;
            overflow-y: auto;
            border: 2px solid #FFD700;
            position: relative;
        }
        
        .help-close {
            position: absolute;
            top: 15px;
            right: 15px;
            background: #F44336;
            color: white;
            width: 45px;
            height: 45px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.8rem;
            cursor: pointer;
            touch-action: manipulation;
        }
        
        .help-title {
            color: #FFD700;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.5rem;
        }
        
        .help-section {
            margin-bottom: 20px;
        }
        
        .help-section h3 {
            color: #4CAF50;
            margin-bottom: 10px;
            font-size: 1.2rem;
        }
        
        .help-section p {
            margin-bottom: 8px;
            line-height: 1.5;
        }
        
        .combo-list {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 8px;
            margin-top: 10px;
        }
        
        .combo-item {
            background-color: rgba(255, 255, 255, 0.1);
            padding: 8px;
            border-radius: 8px;
            font-size: 0.85rem;
        }
        
        /* Адаптация под очень маленькие экраны */
        @media (max-width: 380px) {
            .card {
                width: 52px;
                height: 74px;
            }
            
            .card-center {
                font-size: 1.5rem;
            }
            
            .card-top, .card-bottom {
                font-size: 0.8rem;
            }
            
            .player-cards {
                min-height: 85px;
            }
            
            .action-btn {
                padding: 16px 8px;
                font-size: 1rem;
                min-height: 55px;
            }
            
            h1 {
                font-size: 1.5rem;
            }
            
            .bet-slider {
                height: 36px;
            }
            
            .bet-slider::-webkit-slider-thumb {
                width: 40px;
                height: 40px;
            }
        }
        
        @media (max-height: 700px) {
            .card {
                width: 50px;
                height: 70px;
            }
            
            .player-cards {
                min-height: 80px;
            }
            
            .action-btn {
                padding: 14px 6px;
                min-height: 50px;
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
    </style>
</head>
<body>
    <div class="container">
        <h1>♠️ Мобильный Покер ♥️</h1>
        
        <div class="game-info">
            <div class="info-item">
                <span class="info-label">Ваш баланс</span>
                <span id="player-balance" class="info-value">1000</span>
            </div>
            <div class="info-item">
                <span class="info-label">Банк бота</span>
                <span id="bot-balance" class="info-value">1000</span>
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
        
        <div class="stage-indicator" id="stage">Pre-flop</div>
        
        <!-- Игрок -->
        <div class="player-area" id="player-area">
            <div class="player-header">
                <div class="player-name">Вы</div>
                <div class="player-bet">Ставка: <span id="player-bet">0</span></div>
            </div>
            <div class="player-cards" id="player-cards"></div>
            <div class="player-status" id="player-status">Ожидание...</div>
        </div>
        
        <!-- Игровой стол -->
        <div class="table-area">
            <div class="pot-display">Банк: <span id="pot">0</span></div>
            <div class="community-cards" id="community-cards"></div>
        </div>
        
        <!-- Бот -->
        <div class="player-area" id="bot-area">
            <div class="player-header">
                <div class="player-name">Дилер-бот</div>
                <div class="player-bet">Ставка: <span id="bot-bet">0</span></div>
            </div>
            <div class="player-cards" id="bot-cards"></div>
            <div class="player-status" id="bot-status">Ожидание...</div>
        </div>
        
        <!-- Управление -->
        <div class="controls">
            <div class="bet-control">
                <div>Размер ставки:</div>
                <div class="bet-slider-container">
                    <input type="range" min="10" max="500" value="50" class="bet-slider" id="bet-slider">
                    <div class="bet-amount" id="bet-amount">50</div>
                </div>
            </div>
            
            <div class="action-buttons">
                <button class="action-btn check-btn" id="check-btn" disabled>Проверка</button>
                <button class="action-btn call-btn" id="call-btn" disabled>Колл</button>
                <button class="action-btn raise-btn" id="raise-btn" disabled>Поднять</button>
                <button class="action-btn fold-btn" id="fold-btn" disabled>Фолд</button>
                <button class="action-btn start-btn" id="start-btn">Начать игру</button>
                <button class="action-btn next-round-btn" id="next-round-btn" disabled>След. раунд</button>
            </div>
            
            <div class="game-log" id="game-log">
                <div class="log-entry">Готов к игре. Нажмите "Начать игру"</div>
            </div>
        </div>
    </div>
    
    <!-- Кнопка помощи -->
    <div class="help-toggle" id="help-toggle">?</div>
    
    <!-- Оверлей помощи -->
    <div class="help-overlay" id="help-overlay">
        <div class="help-content">
            <div class="help-close" id="help-close">×</div>
            <h2 class="help-title">Правила игры</h2>
            
            <div class="help-section">
                <h3>Как играть</h3>
                <p>1. Нажмите "Начать игру"</p>
                <p>2. Делайте ставки: Проверка, Колл, Поднять или Фолд</p>
                <p>3. Используйте слайдер для выбора размера ставки</p>
                <p>4. Цель: собрать лучшую покерную комбинацию из 5 карт</p>
            </div>
            
            <div class="help-section">
                <h3>Стадии игры</h3>
                <p><strong>Pre-flop:</strong> Раздаются 2 карты каждому</p>
                <p><strong>Flop:</strong> Выкладываются 3 общие карты</p>
                <p><strong>Turn:</strong> Выкладывается 4-я общая карта</p>
                <p><strong>River:</strong> Выкладывается 5-я общая карта</p>
                <p><strong>Showdown:</strong> Вскрытие карт</p>
            </div>
            
            <div class="help-section">
                <h3>Комбинации покера</h3>
                <div class="combo-list">
                    <div class="combo-item">Роял-флэш</div>
                    <div class="combo-item">Стрит-флэш</div>
                    <div class="combo-item">Каре</div>
                    <div class="combo-item">Фулл-хаус</div>
                    <div class="combo-item">Флэш</div>
                    <div class="combo-item">Стрит</div>
                    <div class="combo-item">Тройка</div>
                    <div class="combo-item">Две пары</div>
                    <div class="combo-item">Пара</div>
                    <div class="combo-item">Старшая карта</div>
                </div>
            </div>
            
            <div class="help-section">
                <h3>Управление на телефоне</h3>
                <p>• Кнопки увеличены для удобного нажатия</p>
                <p>• Свайп влево/вправо по слайдеру для изменения ставки</p>
                <p>• Все элементы адаптированы под мобильный экран</p>
            </div>
        </div>
    </div>

    <script>
        // Упрощенная версия игры для мобильных устройств
        const gameState = {
            player: { balance: 1000, bet: 0, hand: [], folded: false, isActive: false },
            bot: { balance: 1000, bet: 0, hand: [], folded: false, isActive: false, bluffChance: 0.3 },
            deck: [],
            communityCards: [],
            pot: 0,
            round: 1,
            stage: 'preflop',
            currentBet: 10,
            smallBlind: 10,
            dealer: 'bot',
            gameActive: false
        };
        
        // DOM элементы
        const playerBalanceEl = document.getElementById('player-balance');
        const botBalanceEl = document.getElementById('bot-balance');
        const roundEl = document.getElementById('round');
        const currentBetEl = document.getElementById('current-bet');
        const stageEl = document.getElementById('stage');
        const playerBetEl = document.getElementById('player-bet');
        const playerCardsEl = document.getElementById('player-cards');
        const playerStatusEl = document.getElementById('player-status');
        const playerArea = document.getElementById('player-area');
        const botBetEl = document.getElementById('bot-bet');
        const botCardsEl = document.getElementById('bot-cards');
        const botStatusEl = document.getElementById('bot-status');
        const botArea = document.getElementById('bot-area');
        const potEl = document.getElementById('pot');
        const communityCardsEl = document.getElementById('community-cards');
        const gameLog = document.getElementById('game-log');
        const betSlider = document.getElementById('bet-slider');
        const betAmountEl = document.getElementById('bet-amount');
        const checkBtn = document.getElementById('check-btn');
        const callBtn = document.getElementById('call-btn');
        const raiseBtn = document.getElementById('raise-btn');
        const foldBtn = document.getElementById('fold-btn');
        const startBtn = document.getElementById('start-btn');
        const nextRoundBtn = document.getElementById('next-round-btn');
        const helpToggle = document.getElementById('help-toggle');
        const helpOverlay = document.getElementById('help-overlay');
        const helpClose = document.getElementById('help-close');
        
        // Масти и значения
        const suits = ['♠️', '♥️', '♦️', '♣️'];
        const values = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        
        // Инициализация
        function init() {
            updateUI();
            createDeck();
            
            // Исправляем обработчики событий для мобильных
            setupMobileEvents();
            
            // Помощь
            helpToggle.addEventListener('click', () => helpOverlay.style.display = 'flex');
            helpClose.addEventListener('click', () => helpOverlay.style.display = 'none');
            
            // Закрытие помощи по клику вне окна
            helpOverlay.addEventListener('click', (e) => {
                if (e.target === helpOverlay) {
                    helpOverlay.style.display = 'none';
                }
            });
            
            addLog("Готов к игре на мобильном!");
        }
        
        // Настройка мобильных событий
        function setupMobileEvents() {
            // Начало игры
            startBtn.addEventListener('click', startGame);
            startBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                startGame();
            });
            
            // Следующий раунд
            nextRoundBtn.addEventListener('click', nextRound);
            nextRoundBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                nextRound();
            });
            
            // Действия игрока
            checkBtn.addEventListener('click', playerCheck);
            checkBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                playerCheck();
            });
            
            callBtn.addEventListener('click', playerCall);
            callBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                playerCall();
            });
            
            raiseBtn.addEventListener('click', playerRaise);
            raiseBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                playerRaise();
            });
            
            foldBtn.addEventListener('click', playerFold);
            foldBtn.addEventListener('touchstart', function(e) {
                e.preventDefault();
                playerFold();
            });
            
            // Слайдер ставки
            betSlider.addEventListener('input', function() {
                betAmountEl.textContent = this.value;
            });
            
            // Также обрабатываем change для мобильных
            betSlider.addEventListener('change', function() {
                betAmountEl.textContent = this.value;
            });
            
            // Обработка касания слайдера
            betSlider.addEventListener('touchstart', function(e) {
                e.stopPropagation();
            });
            
            betSlider.addEventListener('touchmove', function(e) {
                e.stopPropagation();
            });
            
            // Добавляем вибрацию для кнопок (если поддерживается)
            function vibrate() {
                if ('vibrate' in navigator) {
                    navigator.vibrate(30);
                }
            }
            
            // Добавляем вибрацию ко всем кнопкам
            const allButtons = document.querySelectorAll('.action-btn:not(:disabled), .help-toggle, .help-close');
            allButtons.forEach(btn => {
                btn.addEventListener('touchstart', vibrate);
            });
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
            if (gameState.gameActive) return;
            
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
            
            if (gameState.dealer === 'bot') {
                gameState.player.isActive = true;
                playerArea.classList.add('active');
                addLog("Ваш ход!");
            } else {
                gameState.bot.isActive = true;
                botArea.classList.add('active');
                setTimeout(botAction, 1000);
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
            gameState.bot.hand = [];
            gameState.communityCards = [];
            
            for (let i = 0; i < 2; i++) {
                gameState.player.hand.push(gameState.deck.pop());
                gameState.bot.hand.push(gameState.deck.pop());
            }
            
            renderCards();
        }
        
        // Установка блайндов
        function setBlinds() {
            const smallBlind = gameState.smallBlind;
            const bigBlind = smallBlind * 2;
            
            if (gameState.dealer === 'bot') {
                makeBet(gameState.player, bigBlind);
                makeBet(gameState.bot, smallBlind);
                addLog(`Вы: большой блайнд (${bigBlind})`);
            } else {
                makeBet(gameState.bot, bigBlind);
                makeBet(gameState.player, smallBlind);
                addLog(`Бот: большой блайнд (${bigBlind})`);
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
                endPlayerTurn();
            } else {
                addLog("Нельзя проверять");
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
                addLog("Недостаточно средств!");
                return;
            }
            
            makeBet(gameState.player, callAmount);
            addLog(`Вы делаете колл (${callAmount})`);
            endPlayerTurn();
        }
        
        function playerRaise() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            const raiseAmount = parseInt(betSlider.value);
            const totalBet = gameState.player.bet + raiseAmount;
            
            if (totalBet <= gameState.currentBet) {
                addLog("Слишком маленькая ставка!");
                return;
            }
            
            if (raiseAmount > gameState.player.balance) {
                addLog("Недостаточно средств!");
                return;
            }
            
            makeBet(gameState.player, raiseAmount);
            addLog(`Вы повышаете на ${raiseAmount}`);
            gameState.currentBet = totalBet;
            endPlayerTurn();
        }
        
        function playerFold() {
            if (!gameState.player.isActive || gameState.player.folded) return;
            
            gameState.player.folded = true;
            addLog("Вы сбрасываете карты");
            endGame();
        }
        
        // Завершение хода игрока
        function endPlayerTurn() {
            gameState.player.isActive = false;
            playerArea.classList.remove('active');
            
            if (checkForRoundEnd()) return;
            
            gameState.bot.isActive = true;
            botArea.classList.add('active');
            setTimeout(botAction, 1000);
        }
        
        // Действие бота
        function botAction() {
            if (!gameState.bot.isActive || gameState.bot.folded) return;
            
            const handStrength = evaluateHandStrength();
            const callAmount = gameState.currentBet - gameState.bot.bet;
            const willBluff = Math.random() < gameState.bot.bluffChance;
            
            let action;
            
            if (callAmount === 0) {
                if (handStrength > 0.6 || willBluff) {
                    const raiseAmount = Math.min(gameState.currentBet * 2, gameState.bot.balance);
                    action = { type: 'raise', amount: raiseAmount };
                } else {
                    action = { type: 'check' };
                }
            } else {
                if (handStrength > 0.7 || (willBluff && handStrength > 0.3)) {
                    const raiseAmount = Math.min(callAmount * 2, gameState.bot.balance);
                    action = { type: 'raise', amount: raiseAmount };
                } else if (handStrength > 0.3 || callAmount < gameState.bot.balance * 0.2) {
                    action = { type: 'call' };
                } else {
                    action = { type: 'fold' };
                }
            }
            
            switch (action.type) {
                case 'check':
                    addLog("Бот проверяет");
                    break;
                case 'call':
                    makeBet(gameState.bot, callAmount);
                    addLog(`Бот делает колл (${callAmount})`);
                    break;
                case 'raise':
                    makeBet(gameState.bot, action.amount);
                    gameState.currentBet = gameState.bot.bet;
                    addLog(`Бот повышает на ${action.amount}`);
                    break;
                case 'fold':
                    gameState.bot.folded = true;
                    addLog("Бот сбрасывает карты");
                    endGame();
                    return;
            }
            
            gameState.bot.isActive = false;
            botArea.classList.remove('active');
            
            if (checkForRoundEnd()) return;
            
            gameState.player.isActive = true;
            playerArea.classList.add('active');
            updateUI();
        }
        
        // Проверка окончания раунда
        function checkForRoundEnd() {
            if (gameState.player.folded || gameState.bot.folded) {
                endGame();
                return true;
            }
            
            if (gameState.player.bet === gameState.bot.bet && gameState.player.bet >= gameState.currentBet) {
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
            gameState.bot.bet = 0;
            gameState.currentBet = 0;
            
            if (gameState.dealer === 'bot') {
                gameState.player.isActive = true;
                playerArea.classList.add('active');
            } else {
                gameState.bot.isActive = true;
                botArea.classList.add('active');
                setTimeout(botAction, 1000);
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
            gameState.bot.isActive = false;
            playerArea.classList.remove('active');
            botArea.classList.remove('active');
            
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
            } else if (winner === 'bot') {
                gameState.bot.balance += gameState.pot;
                addLog(`🤖 Бот выиграл ${gameState.pot} монет`);
                botArea.classList.add('winner');
            } else if (winner === 'split') {
                const halfPot = Math.floor(gameState.pot / 2);
                gameState.player.balance += halfPot;
                gameState.bot.balance += halfPot;
                addLog(`🤝 Ничья! Банк разделен`);
            }
            
            // Показ карт бота
            showBotCards();
            
            // Обновление UI
            updateUI();
            
            // Включение кнопки следующего раунда
            nextRoundBtn.disabled = false;
            
            // Смена дилера
            gameState.dealer = gameState.dealer === 'bot' ? 'player' : 'bot';
            
            addLog("Раунд завершен");
        }
        
        // Определение победителя (упрощенная версия)
        function determineWinner() {
            if (gameState.player.folded) return 'bot';
            if (gameState.bot.folded) return 'player';
            
            // Упрощенная оценка для мобильной версии
            const playerScore = evaluateSimpleHand(gameState.player.hand, gameState.communityCards);
            const botScore = evaluateSimpleHand(gameState.bot.hand, gameState.communityCards);
            
            if (playerScore > botScore) return 'player';
            if (botScore > playerScore) return 'bot';
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
            
            if (pairs === 2) score += 1; // Две пары
            if (pairs === 1 && score === 4) score += 2; // Фулл-хаус (упрощенно)
            
            // Добавляем вес по старшим картам
            const sortedCards = allCards.sort((a, b) => b.numericValue - a.numericValue);
            score += sortedCards[0].numericValue * 0.01;
            
            return score;
        }
        
        // Оценка силы руки для бота (упрощенная)
        function evaluateHandStrength() {
            if (gameState.communityCards.length === 0) {
                // Префлоп
                const card1 = gameState.bot.hand[0];
                const card2 = gameState.bot.hand[1];
                
                if (card1.value === card2.value) return 0.7; // Пара
                if (card1.numericValue > 12 || card2.numericValue > 12) return 0.6; // Высокие карты
                if (Math.abs(card1.numericValue - card2.numericValue) === 1) return 0.5; // Соседние
                if (card1.suit === card2.suit) return 0.4; // Одна масть
                return 0.3;
            }
            
            // С общими картами
            const score = evaluateSimpleHand(gameState.bot.hand, gameState.communityCards);
            return Math.min(score / 10, 0.9);
        }
        
        // Показ карт бота
        function showBotCards() {
            botCardsEl.innerHTML = '';
            gameState.bot.hand.forEach(card => {
                const cardElement = createCardElement(card);
                botCardsEl.appendChild(cardElement);
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
            
            // Карты бота
            botCardsEl.innerHTML = '';
            gameState.bot.hand.forEach(card => {
                const isBack = gameState.gameActive;
                const cardElement = createCardElement(card, isBack);
                botCardsEl.appendChild(cardElement);
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
            gameState.bot.bet = 0;
            gameState.pot = 0;
            gameState.player.folded = false;
            gameState.bot.folded = false;
            gameState.player.isActive = false;
            gameState.bot.isActive = false;
            gameState.stage = 'preflop';
            gameState.currentBet = 0;
            
            playerArea.classList.remove('winner', 'active');
            botArea.classList.remove('winner', 'active');
            
            createDeck();
        }
        
        // Включение действий игрока
        function enablePlayerActions() {
            checkBtn.disabled = false;
            callBtn.disabled = false;
            raiseBtn.disabled = false;
            foldBtn.disabled = false;
            
            // Обновляем слайдер ставки
            const maxBet = Math.min(gameState.player.balance, 500);
            betSlider.max = maxBet;
            betSlider.value = Math.min(50, maxBet);
            betAmountEl.textContent = betSlider.value;
        }
        
        // Отключение действий игрока
        function disablePlayerActions() {
            checkBtn.disabled = true;
            callBtn.disabled = true;
            raiseBtn.disabled = true;
            foldBtn.disabled = true;
        }
        
        // Добавление записи в лог
        function addLog(message) {
            const timestamp = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            const logEntry = document.createElement('div');
            logEntry.className = 'log-entry';
            logEntry.textContent = `[${timestamp}] ${message}`;
            
            gameLog.appendChild(logEntry);
            
            // Прокрутка к последнему сообщению
            setTimeout(() => {
                gameLog.scrollTop = gameLog.scrollHeight;
            }, 100);
            
            // Ограничиваем количество записей
            if (gameLog.children.length > 20) {
                gameLog.removeChild(gameLog.firstChild);
            }
        }
        
        // Обновление UI
        function updateUI() {
            playerBalanceEl.textContent = gameState.player.balance;
            botBalanceEl.textContent = gameState.bot.balance;
            playerBetEl.textContent = gameState.player.bet;
            botBetEl.textContent = gameState.bot.bet;
            potEl.textContent = gameState.pot;
            roundEl.textContent = gameState.round;
            currentBetEl.textContent = gameState.currentBet;
            
            // Отображаем стадию игры
            const stageNames = {
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
            botStatusEl.textContent = gameState.bot.isActive ? 'Ходит...' : 
                                     gameState.bot.folded ? 'Сбросил карты' : 'Ожидание';
            
            // Обновляем доступность кнопок
            const canCheck = gameState.currentBet === 0 || gameState.player.bet === gameState.currentBet;
            checkBtn.textContent = canCheck ? 'Проверка' : 'Пропуск';
            
            const callAmount = gameState.currentBet - gameState.player.bet;
            callBtn.textContent = callAmount > 0 ? `Колл (${callAmount})` : 'Колл';
            
            // Обновляем слайдер ставки
            const maxBet = Math.min(gameState.player.balance, 500);
            betSlider.max = maxBet;
            
            // Если ставка больше максимума, уменьшаем ее
            if (parseInt(betSlider.value) > maxBet) {
                betSlider.value = maxBet;
                betAmountEl.textContent = maxBet;
            }
            
            // Если баланс игрока 0, блокируем начало игры
            if (gameState.player.balance < gameState.smallBlind * 2) {
                startBtn.disabled = true;
                startBtn.textContent = 'Недостаточно средств';
            }
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
        
        // Предотвращаем скролл страницы при взаимодействии со слайдером
        document.addEventListener('touchmove', function(e) {
            if (e.target === betSlider) {
                e.preventDefault();
            }
        }, {passive: false});
    </script>
</body>
</html> 
