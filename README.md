
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Telegram Poker</title>
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
            background: linear-gradient(135deg, #1a3c2b, #0f2b1e);
            color: #fff;
            min-height: 100vh;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            overflow-x: hidden;
            touch-action: manipulation;
            padding-top: env(safe-area-inset-top);
            padding-bottom: env(safe-area-inset-bottom);
        }
        
        /* Telegram стили */
        .tg-header {
            background: #3390ec;
            color: white;
            padding: 12px 16px;
            text-align: center;
            font-weight: bold;
            font-size: 1.1rem;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }
        
        .game-container {
            max-width: 100%;
            padding: 15px;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        /* Компактные карты для Telegram */
        .card {
            width: 55px;
            height: 75px;
            background: white;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 6px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.2);
            position: relative;
        }
        
        .card.red { color: #e53935; }
        .card.black { color: #263238; }
        
        .card-value {
            font-size: 0.9rem;
            font-weight: bold;
            line-height: 1;
        }
        
        .card-suit {
            font-size: 1.4rem;
            text-align: center;
            flex-grow: 1;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        /* Компактные игроки */
        .player {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 15px;
            margin-bottom: 10px;
            border: 2px solid transparent;
            transition: all 0.3s;
        }
        
        .player.active {
            border-color: #4CAF50;
            background: rgba(76, 175, 80, 0.1);
        }
        
        .player-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        
        .player-name {
            font-weight: bold;
            font-size: 1rem;
        }
        
        .player-balance {
            color: #FFD700;
            font-weight: bold;
        }
        
        .player-cards {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin: 10px 0;
        }
        
        /* Стол для Telegram */
        .table {
            background: linear-gradient(135deg, #2e7d32, #1b5e20);
            border-radius: 15px;
            padding: 20px 15px;
            text-align: center;
            margin: 10px 0;
            border: 4px solid #8B4513;
        }
        
        .pot {
            font-size: 1.4rem;
            color: #FFD700;
            margin-bottom: 15px;
            font-weight: bold;
        }
        
        /* Кнопки для Telegram */
        .tg-buttons {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-top: 10px;
        }
        
        .tg-button {
            padding: 14px 10px;
            border: none;
            border-radius: 12px;
            font-weight: bold;
            font-size: 0.95rem;
            cursor: pointer;
            transition: all 0.2s;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .tg-button:active {
            transform: scale(0.95);
        }
        
        .tg-button-primary {
            background: #3390ec;
            color: white;
            grid-column: span 2;
        }
        
        .tg-button-success {
            background: #4CAF50;
            color: white;
        }
        
        .tg-button-warning {
            background: #FF9800;
            color: white;
        }
        
        .tg-button-danger {
            background: #F44336;
            color: white;
        }
        
        /* Мобильная адаптация */
        @media (max-height: 600px) {
            .card {
                width: 48px;
                height: 65px;
            }
            
            .card-suit {
                font-size: 1.2rem;
            }
            
            .tg-button {
                padding: 12px 8px;
                font-size: 0.9rem;
            }
        }
        
        /* Анимации */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.3s ease-out;
        }
        
        /* Скрытие карт бота */
        .card-back {
            background: linear-gradient(135deg, #1a237e, #283593);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem;
        }
        
        /* Статус бота */
        .bot-status {
            text-align: center;
            color: #aaa;
            font-size: 0.85rem;
            margin-top: 5px;
            min-height: 20px;
        }
        
        /* Индикатор комбинации игрока */
        .player-combination {
            background: rgba(255, 215, 0, 0.1);
            border: 1px solid rgba(255, 215, 0, 0.3);
            border-radius: 8px;
            padding: 6px 10px;
            text-align: center;
            margin-top: 8px;
            font-size: 0.85rem;
            color: #FFD700;
            min-height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .player-combination.highlight {
            animation: glow 1s infinite alternate;
        }
        
        @keyframes glow {
            from { box-shadow: 0 0 5px rgba(255, 215, 0, 0.5); }
            to { box-shadow: 0 0 15px rgba(255, 215, 0, 0.8); }
        }
    </style>
</head>
<body>
    <div class="tg-header">♠️ Телеграм Покер ♥️</div>
    
    <div class="game-container">
        <!-- Статистика -->
        <div style="display: flex; justify-content: space-between; padding: 0 10px;">
            <div>
                <div style="font-size: 0.8rem; color: #aaa;">БАНК</div>
                <div style="font-size: 1.3rem; font-weight: bold; color: #FFD700;" id="pot">0</div>
            </div>
            <div>
                <div style="font-size: 0.8rem; color: #aaa;">РАУНД</div>
                <div style="font-size: 1.3rem; font-weight: bold; color: #FFD700;" id="round">1</div>
            </div>
        </div>
        
        <!-- Статус игры -->
        <div style="background: rgba(51, 144, 236, 0.1); padding: 10px; border-radius: 10px; text-align: center; margin: 5px 10px; font-weight: bold; color: #3390ec;" id="game-status">
            Нажмите "ИГРАТЬ"
        </div>
        
        <!-- Бот (без комбинации) -->
        <div class="player" id="bot-player">
            <div class="player-header">
                <div class="player-name">🤖 ДИЛЕР</div>
                <div class="player-balance" id="bot-balance">1000</div>
            </div>
            <div class="player-cards" id="bot-cards"></div>
            <!-- У бота нет показа комбинации -->
            <div class="bot-status" id="bot-status">Карты скрыты</div>
        </div>
        
        <!-- Стол -->
        <div class="table">
            <div class="pot">Банк: <span id="pot-value">0</span></div>
            <div style="display: flex; justify-content: center; gap: 8px; flex-wrap: wrap;" id="community-cards"></div>
        </div>
        
        <!-- Игрок (с комбинацией) -->
        <div class="player" id="player-player">
            <div class="player-header">
                <div class="player-name">🎮 ВЫ</div>
                <div class="player-balance" id="player-balance">1000</div>
            </div>
            <div class="player-cards" id="player-cards"></div>
            <!-- Только у игрока показывается комбинация -->
            <div class="player-combination" id="player-combination"></div>
        </div>
        
        <!-- Управление -->
        <div style="padding: 0 15px;">
            <!-- Слайдер ставки -->
            <div style="background: rgba(0, 0, 0, 0.3); padding: 15px; border-radius: 12px; margin-bottom: 10px;">
                <div style="text-align: center; margin-bottom: 10px;">СТАВКА: <span style="color: #FFD700; font-weight: bold;" id="bet-amount">50</span></div>
                <input type="range" min="10" max="500" value="50" style="width: 100%; height: 30px;" id="bet-slider">
            </div>
            
            <!-- Кнопки действий -->
            <div class="tg-buttons">
                <button class="tg-button tg-button-primary" id="play-btn">🎮 НАЧАТЬ ИГРУ</button>
                <button class="tg-button tg-button-success" id="check-call-btn" disabled>✓ ПРОВЕРКА</button>
                <button class="tg-button tg-button-warning" id="raise-btn" disabled>↑ ПОДНЯТЬ</button>
                <button class="tg-button tg-button-danger" id="fold-btn" disabled>✗ ФОЛД</button>
            </div>
            
            <!-- Лог игры -->
            <div style="background: rgba(0, 0, 0, 0.4); border-radius: 10px; padding: 12px; margin-top: 15px; max-height: 100px; overflow-y: auto; font-size: 0.8rem;">
                <div id="game-log">
                    <div style="padding: 5px 0; border-bottom: 1px solid rgba(255, 255, 255, 0.1);">Добро пожаловать в покер!</div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Telegram Web App API
        let TelegramWebApp = {};
        
        // Проверяем, находимся ли мы в Telegram
        function isTelegramWebApp() {
            return window.Telegram && window.Telegram.WebApp;
        }
        
        // Инициализация Telegram Web App
        function initTelegramWebApp() {
            if (isTelegramWebApp()) {
                TelegramWebApp = window.Telegram.WebApp;
                
                // Расширяем приложение на весь экран
                TelegramWebApp.expand();
                
                // Устанавливаем цвет фона
                TelegramWebApp.setBackgroundColor('#0f2b1e');
                
                // Устанавливаем цвет кнопок
                TelegramWebApp.setHeaderColor('#3390ec');
                
                addLog('Telegram Web App загружен');
            }
        }
        
        // Инициализация игры
        const game = {
            player: { balance: 1000, bet: 0, hand: [], folded: false, isActive: false },
            bot: { balance: 1000, bet: 0, hand: [], folded: false, isActive: false, bluff: 0.3 },
            deck: [],
            communityCards: [],
            pot: 0,
            round: 1,
            stage: 'preflop',
            currentBet: 10,
            gameActive: false,
            dealer: 'bot'
        };
        
        // Элементы интерфейса
        const elements = {
            playerBalance: document.getElementById('player-balance'),
            botBalance: document.getElementById('bot-balance'),
            pot: document.getElementById('pot'),
            potValue: document.getElementById('pot-value'),
            round: document.getElementById('round'),
            gameStatus: document.getElementById('game-status'),
            playerCards: document.getElementById('player-cards'),
            botCards: document.getElementById('bot-cards'),
            communityCards: document.getElementById('community-cards'),
            playerCombination: document.getElementById('player-combination'),
            botStatus: document.getElementById('bot-status'),
            betSlider: document.getElementById('bet-slider'),
            betAmount: document.getElementById('bet-amount'),
            playBtn: document.getElementById('play-btn'),
            checkCallBtn: document.getElementById('check-call-btn'),
            raiseBtn: document.getElementById('raise-btn'),
            foldBtn: document.getElementById('fold-btn'),
            gameLog: document.getElementById('game-log'),
            playerElement: document.getElementById('player-player'),
            botElement: document.getElementById('bot-player')
        };
        
        // Масти и карты
        const suits = ['♠️', '♥️', '♦️', '♣️'];
        const values = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        
        // Инициализация
        function init() {
            createDeck();
            setupEventListeners();
            updateUI();
            addLog('Готов к игре!');
            
            // Инициализируем Telegram Web App
            initTelegramWebApp();
        }
        
        // Создание колоды
        function createDeck() {
            game.deck = [];
            for (let suit of suits) {
                for (let value of values) {
                    game.deck.push({
                        suit,
                        value,
                        numeric: values.indexOf(value) + 2,
                        color: suit === '♥️' || suit === '♦️' ? 'red' : 'black'
                    });
                }
            }
        }
        
        // Настройка событий
        function setupEventListeners() {
            // Кнопки
            elements.playBtn.addEventListener('click', startGame);
            elements.playBtn.addEventListener('touchstart', (e) => {
                e.preventDefault();
                startGame();
            });
            
            elements.checkCallBtn.addEventListener('click', () => {
                if (game.currentBet === 0 || game.player.bet === game.currentBet) {
                    playerCheck();
                } else {
                    playerCall();
                }
            });
            
            elements.raiseBtn.addEventListener('click', playerRaise);
            elements.foldBtn.addEventListener('click', playerFold);
            
            // Слайдер
            elements.betSlider.addEventListener('input', () => {
                elements.betAmount.textContent = elements.betSlider.value;
            });
            
            // Эффект нажатия для всех кнопок
            document.querySelectorAll('.tg-button').forEach(btn => {
                btn.addEventListener('touchstart', function() {
                    this.style.transform = 'scale(0.95)';
                });
                
                btn.addEventListener('touchend', function() {
                    this.style.transform = '';
                });
            });
        }
        
        // Начало игры
        function startGame() {
            if (game.gameActive) return;
            
            resetGame();
            game.gameActive = true;
            shuffleDeck();
            dealCards();
            setBlinds();
            updateUI();
            enableControls();
            
            if (game.dealer === 'bot') {
                game.player.isActive = true;
                elements.playerElement.classList.add('active');
                setStatus('Ваш ход!');
                addLog('Ваш ход. Что делаете?');
            } else {
                game.bot.isActive = true;
                elements.botElement.classList.add('active');
                setStatus('Ход дилера...');
                setTimeout(botTurn, 1000);
            }
            
            elements.playBtn.disabled = true;
            elements.playBtn.textContent = 'ИГРА ИДЁТ';
            addLog(`🎲 Раунд ${game.round} начался!`);
        }
        
        // Сброс игры
        function resetGame() {
            game.player.bet = 0;
            game.bot.bet = 0;
            game.pot = 0;
            game.player.folded = false;
            game.bot.folded = false;
            game.player.isActive = false;
            game.bot.isActive = false;
            game.stage = 'preflop';
            game.currentBet = 10;
            
            elements.playerElement.classList.remove('active', 'winner');
            elements.botElement.classList.remove('active', 'winner');
            elements.playerCombination.textContent = '';
            
            createDeck();
        }
        
        // Перемешивание
        function shuffleDeck() {
            for (let i = game.deck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [game.deck[i], game.deck[j]] = [game.deck[j], game.deck[i]];
            }
        }
        
        // Раздача
        function dealCards() {
            game.player.hand = [];
            game.bot.hand = [];
            game.communityCards = [];
            
            for (let i = 0; i < 2; i++) {
                game.player.hand.push(game.deck.pop());
                game.bot.hand.push(game.deck.pop());
            }
            
            renderCards();
        }
        
        // Блайнды
        function setBlinds() {
            const small = 10;
            const big = 20;
            
            if (game.dealer === 'bot') {
                placeBet(game.player, big);
                placeBet(game.bot, small);
                addLog(`Вы: большой блайнд (${big})`);
            } else {
                placeBet(game.bot, big);
                placeBet(game.player, small);
                addLog(`Дилер: большой блайнд (${big})`);
            }
            
            game.currentBet = big;
        }
        
        // Ставка
        function placeBet(player, amount) {
            const bet = Math.min(amount, player.balance);
            player.balance -= bet;
            player.bet += bet;
            game.pot += bet;
            if (bet > game.currentBet) game.currentBet = bet;
        }
        
        // Действия игрока
        function playerCheck() {
            if (!game.player.isActive) return;
            
            addLog('Вы проверяете');
            nextTurn();
        }
        
        function playerCall() {
            if (!game.player.isActive) return;
            
            const amount = game.currentBet - game.player.bet;
            if (amount <= 0) {
                playerCheck();
                return;
            }
            
            if (amount > game.player.balance) {
                addLog('Недостаточно средств!');
                return;
            }
            
            placeBet(game.player, amount);
            addLog(`Вы делаете колл (${amount})`);
            nextTurn();
        }
        
        function playerRaise() {
            if (!game.player.isActive) return;
            
            const amount = parseInt(elements.betSlider.value);
            const total = game.player.bet + amount;
            
            if (total <= game.currentBet) {
                addLog('Ставка слишком мала!');
                return;
            }
            
            if (amount > game.player.balance) {
                addLog('Недостаточно средств!');
                return;
            }
            
            placeBet(game.player, amount);
            game.currentBet = total;
            addLog(`Вы повышаете ставку на ${amount}`);
            nextTurn();
        }
        
        function playerFold() {
            if (!game.player.isActive) return;
            
            game.player.folded = true;
            addLog('Вы сбрасываете карты');
            endRound();
        }
        
        // Ход бота
        function botTurn() {
            if (!game.bot.isActive) return;
            
            const handStrength = evaluateHand(game.bot.hand, game.communityCards);
            const callAmount = game.currentBet - game.bot.bet;
            const willBluff = Math.random() < game.bot.bluff;
            
            let action;
            
            if (callAmount === 0) {
                if (handStrength > 0.6 || willBluff) {
                    const raise = Math.min(game.currentBet * 2, game.bot.balance);
                    action = { type: 'raise', amount: raise };
                } else {
                    action = { type: 'check' };
                }
            } else {
                if (handStrength > 0.7 || (willBluff && handStrength > 0.3)) {
                    const raise = Math.min(callAmount * 2, game.bot.balance);
                    action = { type: 'raise', amount: raise };
                } else if (handStrength > 0.3 || callAmount < game.bot.balance * 0.2) {
                    action = { type: 'call' };
                } else {
                    action = { type: 'fold' };
                }
            }
            
            switch (action.type) {
                case 'check':
                    addLog('Дилер проверяет');
                    break;
                case 'call':
                    placeBet(game.bot, callAmount);
                    addLog(`Дилер делает колл (${callAmount})`);
                    break;
                case 'raise':
                    placeBet(game.bot, action.amount);
                    game.currentBet = game.bot.bet;
                    addLog(`Дилер повышает на ${action.amount}`);
                    break;
                case 'fold':
                    game.bot.folded = true;
                    addLog('Дилер сбрасывает карты');
                    endRound();
                    return;
            }
            
            game.bot.isActive = false;
            elements.botElement.classList.remove('active');
            
            if (checkRoundEnd()) return;
            
            game.player.isActive = true;
            elements.playerElement.classList.add('active');
            setStatus('Ваш ход!');
            updateUI();
        }
        
        // Следующий ход
        function nextTurn() {
            game.player.isActive = false;
            elements.playerElement.classList.remove('active');
            
            if (checkRoundEnd()) return;
            
            game.bot.isActive = true;
            elements.botElement.classList.add('active');
            setStatus('Ход дилера...');
            
            setTimeout(botTurn, 1500);
        }
        
        // Проверка конца раунда
        function checkRoundEnd() {
            if (game.player.folded || game.bot.folded) {
                endRound();
                return true;
            }
            
            if (game.player.bet === game.bot.bet && game.player.bet >= game.currentBet) {
                nextStage();
                return true;
            }
            
            return false;
        }
        
        // Следующая стадия
        function nextStage() {
            switch (game.stage) {
                case 'preflop':
                    game.stage = 'flop';
                    addCommunityCards(3);
                    setStatus('Флоп: 3 карты на столе');
                    addLog('Открыт флоп');
                    break;
                case 'flop':
                    game.stage = 'turn';
                    addCommunityCards(1);
                    setStatus('Терн: 4-я карта');
                    addLog('Открыт терн');
                    break;
                case 'turn':
                    game.stage = 'river';
                    addCommunityCards(1);
                    setStatus('Ривер: 5-я карта');
                    addLog('Открыт ривер');
                    break;
                case 'river':
                    game.stage = 'showdown';
                    endRound();
                    return;
            }
            
            game.player.bet = 0;
            game.bot.bet = 0;
            game.currentBet = 0;
            
            if (game.dealer === 'bot') {
                game.player.isActive = true;
                elements.playerElement.classList.add('active');
                setStatus('Ваш ход!');
            } else {
                game.bot.isActive = true;
                elements.botElement.classList.add('active');
                setStatus('Ход дилера...');
                setTimeout(botTurn, 1000);
            }
            
            updateUI();
        }
        
        // Добавление общих карт
        function addCommunityCards(count) {
            for (let i = 0; i < count; i++) {
                if (game.deck.length > 0) {
                    game.communityCards.push(game.deck.pop());
                }
            }
            renderCards();
            updatePlayerCombination();
        }
        
        // Обновление комбинации игрока (только для игрока)
        function updatePlayerCombination() {
            if (game.communityCards.length > 0) {
                const playerScore = evaluateHand(game.player.hand, game.communityCards);
                const combinationName = getCombinationName(playerScore);
                elements.playerCombination.textContent = `Ваша комбинация: ${combinationName}`;
                
                // Подсветка сильных комбинаций
                if (playerScore >= 5) { // Флэш и выше
                    elements.playerCombination.classList.add('highlight');
                    setTimeout(() => {
                        elements.playerCombination.classList.remove('highlight');
                    }, 2000);
                }
            } else {
                elements.playerCombination.textContent = '';
            }
        }
        
        // Окончание раунда
        function endRound() {
            game.gameActive = false;
            game.player.isActive = false;
            game.bot.isActive = false;
            elements.playerElement.classList.remove('active');
            elements.botElement.classList.remove('active');
            
            // Показываем карты бота только в конце
            showBotCards();
            
            // Определяем победителя
            const winner = determineWinner();
            
            if (winner === 'player') {
                game.player.balance += game.pot;
                addLog(`🎉 Вы выиграли ${game.pot} монет!`);
                elements.playerElement.classList.add('winner');
                highlightWinner('player');
            } else if (winner === 'bot') {
                game.bot.balance += game.pot;
                addLog(`🤖 Дилер выиграл ${game.pot} монет`);
                elements.botElement.classList.add('winner');
                highlightWinner('bot');
            } else {
                const half = Math.floor(game.pot / 2);
                game.player.balance += half;
                game.bot.balance += half;
                addLog(`🤝 Ничья! Банк разделён`);
                highlightWinner('both');
            }
            
            game.dealer = game.dealer === 'bot' ? 'player' : 'bot';
            game.round++;
            
            updateUI();
            resetControls();
            
            setStatus('Раунд завершён');
            addLog('Нажмите "НАЧАТЬ ИГРУ" для нового раунда');
        }
        
        // Подсветка победителя
        function highlightWinner(who) {
            if (who === 'player' || who === 'both') {
                elements.playerElement.style.backgroundColor = 'rgba(76, 175, 80, 0.2)';
                setTimeout(() => {
                    elements.playerElement.style.backgroundColor = '';
                }, 3000);
            }
            if (who === 'bot' || who === 'both') {
                elements.botElement.style.backgroundColor = 'rgba(76, 175, 80, 0.2)';
                setTimeout(() => {
                    elements.botElement.style.backgroundColor = '';
                }, 3000);
            }
        }
        
        // Определение победителя
        function determineWinner() {
            if (game.player.folded) return 'bot';
            if (game.bot.folded) return 'player';
            
            const playerScore = evaluateHand(game.player.hand, game.communityCards);
            const botScore = evaluateHand(game.bot.hand, game.communityCards);
            
            // Показываем финальные комбинации
            const playerCombination = getCombinationName(playerScore);
            const botCombination = getCombinationName(botScore);
            
            elements.playerCombination.textContent = `Вы: ${playerCombination}`;
            elements.botStatus.textContent = `Дилер: ${botCombination}`;
            
            if (playerScore > botScore) return 'player';
            if (botScore > playerScore) return 'bot';
            return 'draw';
        }
        
        // Оценка руки
        function evaluateHand(hand, community) {
            const allCards = [...hand, ...community];
            const values = {};
            const suits = {};
            
            allCards.forEach(card => {
                values[card.value] = (values[card.value] || 0) + 1;
                suits[card.suit] = (suits[card.suit] || 0) + 1;
            });
            
            let score = 0;
            let pairs = 0;
            let three = false;
            let four = false;
            
            // Проверяем флэш
            for (const suit in suits) {
                if (suits[suit] >= 5) {
                    score = Math.max(score, 6); // Флэш
                }
            }
            
            for (const value in values) {
                if (values[value] === 2) {
                    pairs++;
                    score = Math.max(score, 2); // Пара
                } else if (values[value] === 3) {
                    three = true;
                    score = Math.max(score, 4); // Тройка
                } else if (values[value] === 4) {
                    four = true;
                    score = 8; // Каре
                }
            }
            
            // Комбинированные комбинации
            if (four) return 8; // Каре
            if (three && pairs > 0) return 7; // Фулл-хаус
            if (score === 6) return 6; // Флэш
            if (pairs === 2) score = Math.max(score, 3); // Две пары
            
            // Проверяем стрит
            const sortedValues = allCards.map(c => c.numeric).sort((a, b) => a - b);
            let straightCount = 1;
            for (let i = 1; i < sortedValues.length; i++) {
                if (sortedValues[i] === sortedValues[i-1] + 1) {
                    straightCount++;
                    if (straightCount >= 5) {
                        score = Math.max(score, 5); // Стрит
                    }
                } else if (sortedValues[i] !== sortedValues[i-1]) {
                    straightCount = 1;
                }
            }
            
            // Добавляем вес по старшей карте
            const sorted = allCards.sort((a, b) => b.numeric - a.numeric);
            return score + (sorted[0].numeric * 0.001);
        }
        
        // Название комбинации
        function getCombinationName(score) {
            const intScore = Math.floor(score);
            if (intScore >= 8) return 'КАРЕ';
            if (intScore >= 7) return 'ФУЛЛ-ХАУС';
            if (intScore >= 6) return 'ФЛЭШ';
            if (intScore >= 5) return 'СТРИТ';
            if (intScore >= 4) return 'ТРОЙКА';
            if (intScore >= 3) return 'ДВЕ ПАРЫ';
            if (intScore >= 2) return 'ПАРА';
            return 'СТАРШАЯ КАРТА';
        }
        
        // Включение управления
        function enableControls() {
            elements.checkCallBtn.disabled = false;
            elements.raiseBtn.disabled = false;
            elements.foldBtn.disabled = false;
            
            const maxBet = Math.min(game.player.balance, 500);
            elements.betSlider.max = maxBet;
            elements.betSlider.value = Math.min(50, maxBet);
            elements.betAmount.textContent = elements.betSlider.value;
        }
        
        // Сброс управления
        function resetControls() {
            elements.playBtn.disabled = false;
            elements.playBtn.textContent = '🎮 НАЧАТЬ ИГРУ';
            elements.checkCallBtn.disabled = true;
            elements.raiseBtn.disabled = true;
            elements.foldBtn.disabled = true;
            
            // Скрываем комбинацию бота
            elements.botStatus.textContent = 'Карты скрыты';
        }
        
        // Показ карт бота
        function showBotCards() {
            elements.botCards.innerHTML = '';
            game.bot.hand.forEach(card => {
                elements.botCards.appendChild(createCardElement(card, false)); // Теперь показываем карты
            });
        }
        
        // Создание карты
        function createCardElement(card, isBack = false) {
            const div = document.createElement('div');
            
            if (isBack) {
                div.className = 'card card-back';
                div.textContent = '🂠';
            } else {
                div.className = `card ${card.color}`;
                div.innerHTML = `
                    <div class="card-value">${card.value}</div>
                    <div class="card-suit">${card.suit}</div>
                    <div class="card-value" style="transform: rotate(180deg);">${card.value}</div>
                `;
            }
            
            return div;
        }
        
        // Отрисовка карт
        function renderCards() {
            // Карты игрока (всегда видны)
            elements.playerCards.innerHTML = '';
            game.player.hand.forEach(card => {
                elements.playerCards.appendChild(createCardElement(card, false));
            });
            
            // Карты бота (скрыты во время игры)
            elements.botCards.innerHTML = '';
            game.bot.hand.forEach(card => {
                const isBack = game.gameActive && game.stage !== 'showdown';
                elements.botCards.appendChild(createCardElement(card, isBack));
            });
            
            // Общие карты
            elements.communityCards.innerHTML = '';
            game.communityCards.forEach(card => {
                elements.communityCards.appendChild(createCardElement(card, false));
            });
        }
        
        // Обновление интерфейса
        function updateUI() {
            elements.playerBalance.textContent = game.player.balance;
            elements.botBalance.textContent = game.bot.balance;
            elements.pot.textContent = game.pot;
            elements.potValue.textContent = game.pot;
            elements.round.textContent = game.round;
            
            const callAmount = game.currentBet - game.player.bet;
            if (callAmount > 0) {
                elements.checkCallBtn.textContent = `✓ КОЛЛ (${callAmount})`;
            } else {
                elements.checkCallBtn.textContent = '✓ ПРОВЕРКА';
            }
            
            const maxBet = Math.min(game.player.balance, 500);
            elements.betSlider.max = maxBet;
            
            if (parseInt(elements.betSlider.value) > maxBet) {
                elements.betSlider.value = maxBet;
                elements.betAmount.textContent = maxBet;
            }
            
            // Обновляем комбинацию игрока
            updatePlayerCombination();
        }
        
        // Установка статуса
        function setStatus(text) {
            elements.gameStatus.textContent = text;
        }
        
        // Добавление в лог
        function addLog(message) {
            const time = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            const entry = document.createElement('div');
            entry.className = 'fade-in';
            entry.style.padding = '5px 0';
            entry.style.borderBottom = '1px solid rgba(255, 255, 255, 0.1)';
            entry.textContent = `[${time}] ${message}`;
            
            elements.gameLog.appendChild(entry);
            
            setTimeout(() => {
                elements.gameLog.scrollTop = elements.gameLog.scrollHeight;
            }, 100);
            
            if (elements.gameLog.children.length > 20) {
                elements.gameLog.removeChild(elements.gameLog.firstChild);
            }
        }
        
        // Запуск игры
        window.addEventListener('DOMContentLoaded', init);
        
        // Предотвращаем стандартные действия на мобильных
        document.addEventListener('touchmove', (e) => {
            if (e.target === elements.betSlider) {
                e.preventDefault();
            }
        }, { passive: false });
    </script>
</body>
</html>
