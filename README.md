<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title> Покер</title>
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
            background: linear-gradient(135deg, #0a1929, #1a3c2b);
            color: #fff;
            min-height: 100vh;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, sans-serif;
            overflow-x: hidden;
            touch-action: manipulation;
            padding: env(safe-area-inset-top) 10px env(safe-area-inset-bottom) 10px;
        }
        
        /* Партиклы на фоне */
        #particles-js {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            pointer-events: none;
        }
        
        /* Заголовок с анимацией */
        .header {
            text-align: center;
            padding: 15px;
            margin-bottom: 15px;
            position: relative;
        }
        
        .title {
            font-size: 2.2rem;
            font-weight: bold;
            color: #FFD700;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
            margin-bottom: 5px;
            animation: titleGlow 2s infinite alternate;
            background: linear-gradient(45deg, #FFD700, #FFA500, #FFD700);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            letter-spacing: 1px;
        }
        
        .subtitle {
            color: #aaa;
            font-size: 0.9rem;
            opacity: 0.8;
        }
        
        /* Статистика с анимацией */
        .stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            background: rgba(0, 0, 0, 0.6);
            padding: 15px;
            border-radius: 15px;
            margin-bottom: 15px;
            border: 2px solid rgba(255, 215, 0, 0.2);
            backdrop-filter: blur(10px);
            animation: statsPulse 3s infinite;
        }
        
        .stat-item {
            text-align: center;
            padding: 5px;
        }
        
        .stat-label {
            font-size: 0.75rem;
            color: #aaa;
            margin-bottom: 3px;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }
        
        .stat-value {
            font-size: 1.3rem;
            font-weight: bold;
            color: #FFD700;
            text-shadow: 0 0 5px rgba(255, 215, 0, 0.5);
        }
        
        /* Игроки с анимацией */
        .player {
            background: linear-gradient(135deg, 
                rgba(255, 255, 255, 0.08), 
                rgba(255, 255, 255, 0.02));
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 15px;
            border: 3px solid transparent;
            transition: all 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(5px);
        }
        
        .player::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, 
                #ff0000, #ff9900, #ffff00, #00ff00, 
                #00ffff, #0000ff, #9900ff);
            border-radius: 22px;
            z-index: -1;
            opacity: 0;
            transition: opacity 0.4s;
        }
        
        .player.winner::before {
            opacity: 1;
            animation: rainbowBorder 3s linear infinite;
        }
        
        .player.active {
            border-color: #4CAF50;
            box-shadow: 0 0 30px rgba(76, 175, 80, 0.6);
            animation: playerActive 1.5s infinite alternate;
            transform: translateY(-5px);
        }
        
        .player-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .player-name {
            font-size: 1.2rem;
            font-weight: bold;
            color: #FFD700;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .player-balance {
            font-size: 1.2rem;
            color: #4CAF50;
            font-weight: bold;
            text-shadow: 0 0 5px rgba(76, 175, 80, 0.5);
        }
        
        .player-cards {
            display: flex;
            justify-content: center;
            gap: 12px;
            margin: 15px 0;
            min-height: 100px;
        }
        
        /* Карты с супер анимациями */
        .card {
            width: 65px;
            height: 90px;
            background: linear-gradient(145deg, #ffffff, #f0f0f0);
            border-radius: 12px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 8px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.3);
            position: relative;
            transform-style: preserve-3d;
            transition: all 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            cursor: pointer;
            overflow: hidden;
        }
        
        .card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: linear-gradient(45deg, transparent 30%, rgba(255, 255, 255, 0.1) 50%, transparent 70%);
            transform: translateX(-100%);
        }
        
        .card:hover::before {
            animation: cardShine 1s;
        }
        
        .card.red {
            color: #e53935;
        }
        
        .card.black {
            color: #263238;
        }
        
        .card-back {
            background: linear-gradient(135deg, #1a237e, #283593);
            justify-content: center;
            align-items: center;
            font-size: 2rem;
            color: white;
            position: relative;
            overflow: hidden;
        }
        
        .card-back::after {
            content: '';
            position: absolute;
            width: 150%;
            height: 150%;
            background: linear-gradient(45deg, 
                transparent 20%, 
                rgba(255, 255, 255, 0.1) 50%, 
                transparent 80%);
            animation: backShine 3s infinite linear;
            transform: translateX(-100%) rotate(45deg);
        }
        
        .card-value {
            font-size: 1rem;
            font-weight: bold;
            line-height: 1;
            z-index: 1;
        }
        
        .card-suit {
            font-size: 2rem;
            text-align: center;
            line-height: 1;
            flex-grow: 1;
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1;
        }
        
        .card-value.bottom {
            transform: rotate(180deg);
        }
        
        /* Анимация переворота карты */
        .card.flip {
            transform: rotateY(180deg);
        }
        
        /* Подсветка карт в комбинации */
        .card.combo-highlight {
            animation: comboGlow 2s infinite alternate;
            transform: translateY(-10px);
            box-shadow: 0 15px 35px rgba(255, 215, 0, 0.6);
            z-index: 10;
        }
        
        /* Стол с анимацией */
        .table {
            background: radial-gradient(circle at center, #2e7d32, #1b5e20);
            border-radius: 25px;
            padding: 25px;
            text-align: center;
            margin: 20px 0;
            border: 6px solid #8B4513;
            position: relative;
            overflow: hidden;
            box-shadow: 
                inset 0 0 50px rgba(0, 0, 0, 0.5),
                0 10px 40px rgba(0, 0, 0, 0.4);
        }
        
        .table::before {
            content: '';
            position: absolute;
            top: 5px;
            left: 5px;
            right: 5px;
            bottom: 5px;
            border: 3px dashed rgba(255, 255, 255, 0.15);
            border-radius: 20px;
            pointer-events: none;
            animation: tableGlow 3s infinite alternate;
        }
        
        .pot {
            font-size: 1.8rem;
            color: #FFD700;
            margin-bottom: 20px;
            font-weight: bold;
            text-shadow: 0 2px 10px rgba(255, 215, 0, 0.7);
            animation: potPulse 2s infinite alternate;
        }
        
        .community-cards {
            display: flex;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
        }
        
        /* Комбинация с анимацией */
        .combination {
            background: rgba(0, 0, 0, 0.7);
            padding: 12px 20px;
            border-radius: 12px;
            margin-top: 15px;
            font-size: 0.95rem;
            color: #FFD700;
            text-align: center;
            border: 2px solid rgba(255, 215, 0, 0.3);
            position: relative;
            overflow: hidden;
            backdrop-filter: blur(10px);
        }
        
        .combination::before {
            content: '';
            position: absolute;
            top: -2px;
            left: -2px;
            right: -2px;
            bottom: -2px;
            background: linear-gradient(45deg, #FFD700, #FFA500, #FFD700);
            border-radius: 14px;
            z-index: -1;
            opacity: 0;
            transition: opacity 0.3s;
        }
        
        .combination.highlight {
            animation: comboTextGlow 1s infinite alternate;
        }
        
        .combination.highlight::before {
            opacity: 0.3;
        }
        
        /* Управление с анимациями */
        .controls {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-top: 20px;
        }
        
        .bet-control {
            background: linear-gradient(135deg, rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.3));
            padding: 20px;
            border-radius: 18px;
            text-align: center;
            border: 2px solid rgba(255, 215, 0, 0.2);
            backdrop-filter: blur(10px);
        }
        
        .bet-slider-container {
            margin-top: 15px;
            position: relative;
        }
        
        .bet-slider {
            width: 100%;
            height: 40px;
            -webkit-appearance: none;
            background: linear-gradient(to right, 
                #4CAF50, #FFC107, #F44336);
            border-radius: 20px;
            outline: none;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .bet-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: linear-gradient(135deg, #FFD700, #FFA500);
            border: 4px solid white;
            cursor: pointer;
            box-shadow: 0 0 20px rgba(255, 215, 0, 0.8);
            transform: scale(1);
            transition: transform 0.2s;
        }
        
        .bet-slider::-webkit-slider-thumb:hover {
            transform: scale(1.1);
        }
        
        .bet-amount {
            font-size: 1.6rem;
            color: #FFD700;
            margin-top: 10px;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(255, 215, 0, 0.7);
            animation: amountPulse 1s infinite alternate;
        }
        
        .action-buttons {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }
        
        .action-btn {
            padding: 20px 15px;
            font-size: 1.1rem;
            font-weight: bold;
            border: none;
            border-radius: 16px;
            cursor: pointer;
            transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            position: relative;
            overflow: hidden;
            min-height: 65px;
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
            transform: translateY(0);
        }
        
        .action-btn::before {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            width: 0;
            height: 0;
            border-radius: 50%;
            background: rgba(255, 255, 255, 0.3);
            transform: translate(-50%, -50%);
            transition: width 0.6s, height 0.6s;
        }
        
        .action-btn:active::before {
            width: 300px;
            height: 300px;
        }
        
        .action-btn:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 25px rgba(0, 0, 0, 0.4);
        }
        
        .action-btn:active {
            transform: translateY(-2px);
        }
        
        .action-btn:disabled {
            opacity: 0.5;
            transform: none;
            cursor: not-allowed;
            box-shadow: none;
        }
        
        .action-btn-primary {
            background: linear-gradient(135deg, #2196F3, #1976D2);
            color: white;
            grid-column: span 2;
        }
        
        .action-btn-success {
            background: linear-gradient(135deg, #4CAF50, #388E3C);
            color: white;
        }
        
        .action-btn-warning {
            background: linear-gradient(135deg, #FF9800, #F57C00);
            color: white;
        }
        
        .action-btn-danger {
            background: linear-gradient(135deg, #F44336, #D32F2F);
            color: white;
        }
        
        /* Эффект нажатия */
        .action-btn:active {
            animation: btnClick 0.3s;
        }
        
        /* Лог с анимацией */
        .game-log {
            background: rgba(0, 0, 0, 0.7);
            border-radius: 15px;
            padding: 15px;
            margin-top: 20px;
            max-height: 120px;
            overflow-y: auto;
            font-size: 0.85rem;
            border: 2px solid rgba(255, 215, 0, 0.2);
            backdrop-filter: blur(10px);
        }
        
        .log-entry {
            padding: 8px 0;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            color: #e0e0e0;
            animation: logEntry 0.5s;
        }
        
        /* Анимации */
        @keyframes titleGlow {
            0% { text-shadow: 0 0 10px rgba(255, 215, 0, 0.5); }
            100% { text-shadow: 0 0 20px rgba(255, 215, 0, 0.8), 0 0 30px rgba(255, 215, 0, 0.6); }
        }
        
        @keyframes statsPulse {
            0%, 100% { box-shadow: 0 0 0 rgba(255, 215, 0, 0); }
            50% { box-shadow: 0 0 15px rgba(255, 215, 0, 0.3); }
        }
        
        @keyframes playerActive {
            0% { box-shadow: 0 0 20px rgba(76, 175, 80, 0.6); }
            100% { box-shadow: 0 0 40px rgba(76, 175, 80, 0.9); }
        }
        
        @keyframes rainbowBorder {
            0% { filter: hue-rotate(0deg); }
            100% { filter: hue-rotate(360deg); }
        }
        
        @keyframes cardShine {
            0% { transform: translateX(-100%) skewX(-15deg); }
            100% { transform: translateX(200%) skewX(-15deg); }
        }
        
        @keyframes backShine {
            0% { transform: translateX(-100%) rotate(45deg); }
            100% { transform: translateX(100%) rotate(45deg); }
        }
        
        @keyframes comboGlow {
            0% { 
                box-shadow: 0 10px 25px rgba(255, 215, 0, 0.4);
                transform: translateY(-8px) scale(1.05);
            }
            100% { 
                box-shadow: 0 20px 40px rgba(255, 215, 0, 0.8);
                transform: translateY(-12px) scale(1.1);
            }
        }
        
        @keyframes tableGlow {
            0% { border-color: rgba(255, 255, 255, 0.1); }
            100% { border-color: rgba(255, 255, 255, 0.3); }
        }
        
        @keyframes potPulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.05); }
        }
        
        @keyframes comboTextGlow {
            0% { 
                text-shadow: 0 0 10px rgba(255, 215, 0, 0.5);
                box-shadow: 0 0 20px rgba(255, 215, 0, 0.3);
            }
            100% { 
                text-shadow: 0 0 20px rgba(255, 215, 0, 0.8);
                box-shadow: 0 0 40px rgba(255, 215, 0, 0.5);
            }
        }
        
        @keyframes amountPulse {
            0% { transform: scale(1); }
            100% { transform: scale(1.1); }
        }
        
        @keyframes btnClick {
            0% { transform: scale(1); }
            50% { transform: scale(0.95); }
            100% { transform: scale(1); }
        }
        
        @keyframes logEntry {
            from { 
                opacity: 0; 
                transform: translateX(-20px); 
            }
            to { 
                opacity: 1; 
                transform: translateX(0); 
            }
        }
        
        @keyframes cardDeal {
            0% { 
                opacity: 0;
                transform: translateY(-50px) rotate(-10deg);
            }
            100% { 
                opacity: 1;
                transform: translateY(0) rotate(0);
            }
        }
        
        @keyframes chipFall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 0;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translateY(50px) rotate(360deg);
                opacity: 0;
            }
        }
        
        /* Эффект раздачи карт */
        .card.deal-animation {
            animation: cardDeal 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275) forwards;
        }
        
        /* Адаптация */
        @media (max-width: 380px) {
            .card {
                width: 55px;
                height: 80px;
            }
            
            .card-suit {
                font-size: 1.6rem;
            }
            
            .action-btn {
                padding: 18px 12px;
                font-size: 1rem;
            }
            
            .title {
                font-size: 1.8rem;
            }
        }
        
        /* Статус игры */
        .game-status {
            text-align: center;
            padding: 15px;
            background: linear-gradient(135deg, rgba(33, 150, 243, 0.3), rgba(33, 150, 243, 0.1));
            border-radius: 15px;
            font-weight: bold;
            color: #2196F3;
            margin-bottom: 15px;
            border: 2px solid #2196F3;
            animation: statusPulse 2s infinite alternate;
            backdrop-filter: blur(5px);
        }
        
        @keyframes statusPulse {
            0% { 
                box-shadow: 0 0 10px rgba(33, 150, 243, 0.3);
                transform: scale(1);
            }
            100% { 
                box-shadow: 0 0 20px rgba(33, 150, 243, 0.6);
                transform: scale(1.02);
            }
        }
        
        /* Эффект выигрыша */
        .win-effect {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 1000;
            opacity: 0;
        }
        
        .confetti {
            position: absolute;
            width: 10px;
            height: 20px;
            background: #FFD700;
            animation: confettiFall 3s linear forwards;
        }
        
        @keyframes confettiFall {
            0% {
                transform: translateY(-100px) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <!-- Частицы на фоне -->
    <div id="particles-js"></div>
    
    <!-- Эффект выигрыша -->
    <div class="win-effect" id="win-effect"></div>
    
    <div style="max-width: 500px; margin: 0 auto; padding: 10px;">
        <!-- Шапка -->
        <div class="header">
            <div class="title">🔥 ЭПИЧНЫЙ ПОКЕР</div>
            <div class="subtitle">Невероятные анимации и эффекты!</div>
        </div>
        
        <!-- Статистика -->
        <div class="stats">
            <div class="stat-item">
                <div class="stat-label">БАНК</div>
                <div class="stat-value" id="pot">0</div>
            </div>
            <div class="stat-item">
                <div class="stat-label">РАУНД</div>
                <div class="stat-value" id="round">1</div>
            </div>
            <div class="stat-item">
                <div class="stat-label">СТАВКА</div>
                <div class="stat-value" id="current-bet">10</div>
            </div>
        </div>
        
        <!-- Статус игры -->
        <div class="game-status" id="game-status">🎮 НАЖМИТЕ "ИГРАТЬ"</div>
        
        <!-- Бот -->
        <div class="player" id="bot-player">
            <div class="player-header">
                <div class="player-name">🤖 ДИЛЕР</div>
                <div class="player-balance" id="bot-balance">1000</div>
            </div>
            <div class="player-cards" id="bot-cards"></div>
            <div class="combination" id="bot-combination"></div>
        </div>
        
        <!-- Стол -->
        <div class="table">
            <div class="pot">Банк: <span id="pot-value">0</span> 🪙</div>
            <div class="community-cards" id="community-cards"></div>
        </div>
        
        <!-- Игрок -->
        <div class="player" id="player-player">
            <div class="player-header">
                <div class="player-name">🎮 ВЫ</div>
                <div class="player-balance" id="player-balance">1000</div>
            </div>
            <div class="player-cards" id="player-cards"></div>
            <div class="combination" id="player-combination"></div>
        </div>
        
        <!-- Управление -->
        <div class="controls">
            <div class="bet-control">
                <div>СТАВКА:</div>
                <div class="bet-slider-container">
                    <input type="range" min="10" max="500" value="50" class="bet-slider" id="bet-slider">
                </div>
                <div class="bet-amount" id="bet-amount">50</div>
            </div>
            
            <div class="action-buttons">
                <button class="action-btn action-btn-primary" id="play-btn">🎮 НАЧАТЬ ИГРУ</button>
                <button class="action-btn action-btn-success" id="check-call-btn" disabled>✓ ПРОВЕРКА</button>
                <button class="action-btn action-btn-warning" id="raise-btn" disabled>↑ ПОДНЯТЬ</button>
                <button class="action-btn action-btn-danger" id="fold-btn" disabled>✗ ФОЛД</button>
            </div>
            
            <div class="game-log" id="game-log">
                <div class="log-entry">Добро пожаловать в эпичный покер! 💫</div>
            </div>
        </div>
    </div>

    <script>
        // Библиотека для частиц
        function initParticles() {
            const canvas = document.createElement('canvas');
            canvas.id = 'particles-canvas';
            canvas.style.position = 'fixed';
            canvas.style.top = '0';
            canvas.style.left = '0';
            canvas.style.width = '100%';
            canvas.style.height = '100%';
            canvas.style.pointerEvents = 'none';
            canvas.style.zIndex = '-1';
            document.getElementById('particles-js').appendChild(canvas);
            
            const ctx = canvas.getContext('2d');
            let particles = [];
            let particleCount = 50;
            
            // Настройка размера canvas
            function resizeCanvas() {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
            }
            
            // Создание частиц
            function createParticles() {
                particles = [];
                for (let i = 0; i < particleCount; i++) {
                    particles.push({
                        x: Math.random() * canvas.width,
                        y: Math.random() * canvas.height,
                        size: Math.random() * 3 + 1,
                        speedX: Math.random() * 0.5 - 0.25,
                        speedY: Math.random() * 0.5 - 0.25,
                        color: `rgba(${Math.random() * 255}, ${Math.random() * 255}, ${Math.random() * 255}, 0.5)`
                    });
                }
            }
            
            // Анимация частиц
            function animateParticles() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                
                particles.forEach(particle => {
                    particle.x += particle.speedX;
                    particle.y += particle.speedY;
                    
                    if (particle.x < 0) particle.x = canvas.width;
                    if (particle.x > canvas.width) particle.x = 0;
                    if (particle.y < 0) particle.y = canvas.height;
                    if (particle.y > canvas.height) particle.y = 0;
                    
                    ctx.beginPath();
                    ctx.arc(particle.x, particle.y, particle.size, 0, Math.PI * 2);
                    ctx.fillStyle = particle.color;
                    ctx.fill();
                });
                
                requestAnimationFrame(animateParticles);
            }
            
            // Инициализация
            resizeCanvas();
            createParticles();
            animateParticles();
            
            window.addEventListener('resize', () => {
                resizeCanvas();
                createParticles();
            });
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
            currentBet: document.getElementById('current-bet'),
            gameStatus: document.getElementById('game-status'),
            playerCards: document.getElementById('player-cards'),
            botCards: document.getElementById('bot-cards'),
            communityCards: document.getElementById('community-cards'),
            playerCombination: document.getElementById('player-combination'),
            botCombination: document.getElementById('bot-combination'),
            betSlider: document.getElementById('bet-slider'),
            betAmount: document.getElementById('bet-amount'),
            playBtn: document.getElementById('play-btn'),
            checkCallBtn: document.getElementById('check-call-btn'),
            raiseBtn: document.getElementById('raise-btn'),
            foldBtn: document.getElementById('fold-btn'),
            gameLog: document.getElementById('game-log'),
            playerElement: document.getElementById('player-player'),
            botElement: document.getElementById('bot-player'),
            winEffect: document.getElementById('win-effect')
        };
        
        // Масти и карты
        const suits = ['♠️', '♥️', '♦️', '♣️'];
        const values = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A'];
        
        // Инициализация
        function init() {
            createDeck();
            setupEventListeners();
            updateUI();
            initParticles();
            addLog('💫 Добро пожаловать в Эпичный Покер!');
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
        }
        
        // Создание конфессий
        function createConfetti(count = 100) {
            elements.winEffect.innerHTML = '';
            elements.winEffect.style.opacity = '1';
            
            for (let i = 0; i < count; i++) {
                const confetti = document.createElement('div');
                confetti.className = 'confetti';
                confetti.style.left = Math.random() * 100 + '%';
                confetti.style.background = `hsl(${Math.random() * 360}, 100%, 50%)`;
                confetti.style.width = Math.random() * 10 + 5 + 'px';
                confetti.style.height = Math.random() * 15 + 10 + 'px';
                confetti.style.animationDelay = Math.random() * 3 + 's';
                elements.winEffect.appendChild(confetti);
            }
            
            setTimeout(() => {
                elements.winEffect.style.opacity = '0';
                setTimeout(() => {
                    elements.winEffect.innerHTML = '';
                }, 3000);
            }, 3000);
        }
        
        // Анимация переворота карты
        function flipCard(cardElement, callback) {
            cardElement.classList.add('flip');
            setTimeout(() => {
                if (callback) callback();
                setTimeout(() => {
                    cardElement.classList.remove('flip');
                }, 1000);
            }, 300);
        }
        
        // Подсветка карт в комбинации
        function highlightComboCards(hand, communityCards) {
            // Сбрасываем подсветку
            document.querySelectorAll('.card').forEach(card => {
                card.classList.remove('combo-highlight');
            });
            
            // Определяем сильные комбинации
            const allCards = [...hand, ...communityCards];
            const score = evaluateHand(hand, communityCards);
            
            if (score >= 4) { // Тройка и выше
                setTimeout(() => {
                    // Подсвечиваем все карты
                    document.querySelectorAll('.card').forEach(card => {
                        card.classList.add('combo-highlight');
                    });
                }, 500);
            }
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
            
            // Анимация активного игрока
            if (game.dealer === 'bot') {
                game.player.isActive = true;
                elements.playerElement.classList.add('active');
                setStatus('🎯 ВАШ ХОД!');
                addLog('🎲 Ваш ход. Что делаете?');
            } else {
                game.bot.isActive = true;
                elements.botElement.classList.add('active');
                setStatus('🤖 Ход дилера...');
                setTimeout(botTurn, 1500);
            }
            
            elements.playBtn.disabled = true;
            elements.playBtn.textContent = '⚡ ИГРА ИДЁТ';
            addLog(`🎰 Раунд ${game.round} начался!`);
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
            elements.botCombination.textContent = '';
            
            createDeck();
        }
        
        // Перемешивание
        function shuffleDeck() {
            for (let i = game.deck.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [game.deck[i], game.deck[j]] = [game.deck[j], game.deck[i]];
            }
        }
        
        // Раздача с анимацией
        function dealCards() {
            game.player.hand = [];
            game.bot.hand = [];
            game.communityCards = [];
            
            for (let i = 0; i < 2; i++) {
                game.player.hand.push(game.deck.pop());
                game.bot.hand.push(game.deck.pop());
            }
            
            renderCardsWithAnimation();
        }
        
        // Отрисовка с анимацией
        function renderCardsWithAnimation() {
            // Карты игрока
            elements.playerCards.innerHTML = '';
            game.player.hand.forEach((card, index) => {
                const cardElement = createCardElement(card, false);
                cardElement.style.animationDelay = `${index * 0.2}s`;
                cardElement.classList.add('deal-animation');
                elements.playerCards.appendChild(cardElement);
            });
            
            // Карты бота
            elements.botCards.innerHTML = '';
            game.bot.hand.forEach((card, index) => {
                const cardElement = createCardElement(card, true);
                cardElement.style.animationDelay = `${(index + 2) * 0.2}s`;
                cardElement.classList.add('deal-animation');
                elements.botCards.appendChild(cardElement);
            });
            
            // Общие карты
            elements.communityCards.innerHTML = '';
            game.communityCards.forEach((card, index) => {
                const cardElement = createCardElement(card, false);
                cardElement.style.animationDelay = `${index * 0.3}s`;
                cardElement.classList.add('deal-animation');
                elements.communityCards.appendChild(cardElement);
            });
        }
        
        // Блайнды
        function setBlinds() {
            const small = 10;
            const big = 20;
            
            if (game.dealer === 'bot') {
                placeBet(game.player, big);
                placeBet(game.bot, small);
                addLog(`💎 Вы: большой блайнд (${big})`);
            } else {
                placeBet(game.bot, big);
                placeBet(game.player, small);
                addLog(`🤖 Дилер: большой блайнд (${big})`);
            }
            
            game.currentBet = big;
        }
        
        // Ставка с анимацией
        function placeBet(player, amount) {
            const bet = Math.min(amount, player.balance);
            player.balance -= bet;
            player.bet += bet;
            game.pot += bet;
            if (bet > game.currentBet) game.currentBet = bet;
            
            // Анимация ставки
            if (player === game.player) {
                elements.playerElement.style.transform = 'scale(1.05)';
                setTimeout(() => {
                    elements.playerElement.style.transform = '';
                }, 300);
            }
        }
        
        // Действия игрока
        function playerCheck() {
            if (!game.player.isActive) return;
            
            addLog('✅ Вы проверяете');
            elements.checkCallBtn.style.transform = 'scale(0.9)';
            setTimeout(() => {
                elements.checkCallBtn.style.transform = '';
                nextTurn();
            }, 300);
        }
        
        function playerCall() {
            if (!game.player.isActive) return;
            
            const amount = game.currentBet - game.player.bet;
            if (amount <= 0) {
                playerCheck();
                return;
            }
            
            if (amount > game.player.balance) {
                addLog('💸 Недостаточно средств!');
                return;
            }
            
            placeBet(game.player, amount);
            addLog(`💰 Вы делаете колл (${amount})`);
            elements.checkCallBtn.style.transform = 'scale(0.9)';
            setTimeout(() => {
                elements.checkCallBtn.style.transform = '';
                nextTurn();
            }, 300);
        }
        
        function playerRaise() {
            if (!game.player.isActive) return;
            
            const amount = parseInt(elements.betSlider.value);
            const total = game.player.bet + amount;
            
            if (total <= game.currentBet) {
                addLog('📉 Ставка слишком мала!');
                return;
            }
            
            if (amount > game.player.balance) {
                addLog('💸 Недостаточно средств!');
                return;
            }
            
            placeBet(game.player, amount);
            game.currentBet = total;
            addLog(`🚀 Вы повышаете ставку на ${amount}`);
            elements.raiseBtn.style.transform = 'scale(0.9)';
            setTimeout(() => {
                elements.raiseBtn.style.transform = '';
                nextTurn();
            }, 300);
        }
        
        function playerFold() {
            if (!game.player.isActive) return;
            
            game.player.folded = true;
            addLog('❌ Вы сбрасываете карты');
            elements.foldBtn.style.transform = 'scale(0.9)';
            setTimeout(() => {
                elements.foldBtn.style.transform = '';
                endRound();
            }, 300);
        }
        
        // Ход бота с анимацией
        function botTurn() {
            if (!game.bot.isActive) return;
            
            // Анимация размышления
            elements.botElement.style.transform = 'scale(1.05)';
            setTimeout(() => {
                elements.botElement.style.transform = '';
                
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
                
                // Анимация действия
                switch (action.type) {
                    case 'check':
                        addLog('🤖 Дилер проверяет');
                        break;
                    case 'call':
                        placeBet(game.bot, callAmount);
                        addLog(`🤖 Дилер делает колл (${callAmount})`);
                        break;
                    case 'raise':
                        placeBet(game.bot, action.amount);
                        game.currentBet = game.bot.bet;
                        addLog(`🤖 Дилер повышает на ${action.amount}`);
                        break;
                    case 'fold':
                        game.bot.folded = true;
                        addLog('🤖 Дилер сбрасывает карты');
                        endRound();
                        return;
                }
                
                game.bot.isActive = false;
                elements.botElement.classList.remove('active');
                
                if (checkRoundEnd()) return;
                
                game.player.isActive = true;
                elements.playerElement.classList.add('active');
                setStatus('🎯 ВАШ ХОД!');
                updateUI();
                
            }, 1000);
        }
        
        // Следующий ход
        function nextTurn() {
            game.player.isActive = false;
            elements.playerElement.classList.remove('active');
            
            if (checkRoundEnd()) return;
            
            game.bot.isActive = true;
            elements.botElement.classList.add('active');
            setStatus('🤖 Ход дилера...');
            
            setTimeout(botTurn, 1000);
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
        
        // Следующая стадия с анимацией
        function nextStage() {
            switch (game.stage) {
                case 'preflop':
                    game.stage = 'flop';
                    addCommunityCards(3);
                    setStatus('🃏 ФЛОП: 3 карты на столе');
                    addLog('✨ Открыт флоп');
                    break;
                case 'flop':
                    game.stage = 'turn';
                    addCommunityCards(1);
                    setStatus('🎴 ТЕРН: 4-я карта');
                    addLog('🌟 Открыт терн');
                    break;
                case 'turn':
                    game.stage = 'river';
                    addCommunityCards(1);
                    setStatus('🌊 РИВЕР: 5-я карта');
                    addLog('💫 Открыт ривер');
                    break;
                case 'river':
                    game.stage = 'showdown';
                    endRound();
                    return;
            }
            
            game.player.bet = 0;
            game.bot.bet = 0;
            game.currentBet = 0;
            
            // Анимация смены стадии
            document.querySelectorAll('.card').forEach(card => {
                card.style.transform = 'scale(1.1)';
                setTimeout(() => {
                    card.style.transform = '';
                }, 300);
            });
            
            if (game.dealer === 'bot') {
                game.player.isActive = true;
                elements.playerElement.classList.add('active');
                setStatus('🎯 ВАШ ХОД!');
            } else {
                game.bot.isActive = true;
                elements.botElement.classList.add('active');
                setStatus('🤖 Ход дилера...');
                setTimeout(botTurn, 1500);
            }
            
            updateUI();
        }
        
        // Добавление общих карт с анимацией
        function addCommunityCards(count) {
            setTimeout(() => {
                for (let i = 0; i < count; i++) {
                    if (game.deck.length > 0) {
                        game.communityCards.push(game.deck.pop());
                    }
                }
                renderCardsWithAnimation();
                updateCombinations();
                
                // Подсветка комбинаций
                highlightComboCards(game.player.hand, game.communityCards);
            }, 500);
        }
        
        // Обновление комбинаций с анимацией
        function updateCombinations() {
            if (game.communityCards.length > 0) {
                const playerScore = evaluateHand(game.player.hand, game.communityCards);
                const combinationName = getCombinationName(playerScore);
                elements.playerCombination.textContent = `🎯 ВАША КОМБИНАЦИЯ: ${combinationName}`;
                
                // Анимация для сильных комбинаций
                if (playerScore >= 5) {
                    elements.playerCombination.classList.add('highlight');
                    setTimeout(() => {
                        elements.playerCombination.classList.remove('highlight');
                    }, 3000);
                }
            }
        }
        
        // Окончание раунда с эпичной анимацией
        function endRound() {
            game.gameActive = false;
            game.player.isActive = false;
            game.bot.isActive = false;
            elements.playerElement.classList.remove('active');
            elements.botElement.classList.remove('active');
            
            // Переворачиваем карты бота
            setTimeout(() => {
                showBotCards();
                
                const winner = determineWinner();
                
                if (winner === 'player') {
                    game.player.balance += game.pot;
                    addLog(`🎉 ВЫ ВЫИГРАЛИ ${game.pot} МОНЕТ!`);
                    elements.playerElement.classList.add('winner');
                    createConfetti();
                    highlightWinner('player');
                    setStatus('🏆 ПОБЕДА!');
                } else if (winner === 'bot') {
                    game.bot.balance += game.pot;
                    addLog(`🤖 Дилер выиграл ${game.pot} монет`);
                    elements.botElement.classList.add('winner');
                    highlightWinner('bot');
                    setStatus('😔 Поражение...');
                } else {
                    const half = Math.floor(game.pot / 2);
                    game.player.balance += half;
                    game.bot.balance += half;
                    addLog(`🤝 Ничья! Банк разделён`);
                    highlightWinner('both');
                    setStatus('🤝 НИЧЬЯ!');
                }
                
                game.dealer = game.dealer === 'bot' ? 'player' : 'bot';
                game.round++;
                
                updateUI();
                resetControls();
                
                setTimeout(() => {
                    setStatus('🔄 Раунд завершён');
                    addLog('🎮 Нажмите "НАЧАТЬ ИГРУ" для нового раунда');
                }, 2000);
                
            }, 1000);
        }
        
        // Подсветка победителя
        function highlightWinner(who) {
            if (who === 'player' || who === 'both') {
                elements.playerElement.style.transform = 'scale(1.05)';
                setTimeout(() => {
                    elements.playerElement.style.transform = '';
                }, 1000);
            }
            if (who === 'bot' || who === 'both') {
                elements.botElement.style.transform = 'scale(1.05)';
                setTimeout(() => {
                    elements.botElement.style.transform = '';
                }, 1000);
            }
        }
        
        // Определение победителя
        function determineWinner() {
            if (game.player.folded) return 'bot';
            if (game.bot.folded) return 'player';
            
            const playerScore = evaluateHand(game.player.hand, game.communityCards);
            const botScore = evaluateHand(game.bot.hand, game.communityCards);
            
            const playerCombination = getCombinationName(playerScore);
            const botCombination = getCombinationName(botScore);
            
            elements.playerCombination.textContent = `Вы: ${playerCombination}`;
            elements.botCombination.textContent = `Дилер: ${botCombination}`;
            
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
                    score = Math.max(score, 6);
                }
            }
            
            for (const value in values) {
                if (values[value] === 2) {
                    pairs++;
                    score = Math.max(score, 2);
                } else if (values[value] === 3) {
                    three = true;
                    score = Math.max(score, 4);
                } else if (values[value] === 4) {
                    four = true;
                    score = 8;
                }
            }
            
            if (four) return 8;
            if (three && pairs > 0) return 7;
            if (score === 6) return 6;
            if (pairs === 2) score = Math.max(score, 3);
            
            // Проверяем стрит
            const sortedValues = allCards.map(c => c.numeric).sort((a, b) => a - b);
            let straightCount = 1;
            for (let i = 1; i < sortedValues.length; i++) {
                if (sortedValues[i] === sortedValues[i-1] + 1) {
                    straightCount++;
                    if (straightCount >= 5) {
                        score = Math.max(score, 5);
                    }
                } else if (sortedValues[i] !== sortedValues[i-1]) {
                    straightCount = 1;
                }
            }
            
            return score + (Math.max(...sortedValues) * 0.001);
        }
        
        // Название комбинации
        function getCombinationName(score) {
            const intScore = Math.floor(score);
            if (intScore >= 8) return '🔥 КАРЕ';
            if (intScore >= 7) return '💎 ФУЛЛ-ХАУС';
            if (intScore >= 6) return '💧 ФЛЭШ';
            if (intScore >= 5) return '📏 СТРИТ';
            if (intScore >= 4) return '🎲 ТРОЙКА';
            if (intScore >= 3) return '👥 ДВЕ ПАРЫ';
            if (intScore >= 2) return '🤝 ПАРА';
            return '👑 СТАРШАЯ КАРТА';
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
            setTimeout(() => {
                elements.playBtn.disabled = false;
                elements.playBtn.textContent = '🎮 НАЧАТЬ ИГРУ';
                elements.checkCallBtn.disabled = true;
                elements.raiseBtn.disabled = true;
                elements.foldBtn.disabled = true;
                
                // Убираем победные эффекты
                elements.playerElement.classList.remove('winner');
                elements.botElement.classList.remove('winner');
                elements.playerCombination.classList.remove('highlight');
            }, 3000);
        }
        
        // Показ карт бота
        function showBotCards() {
            elements.botCards.innerHTML = '';
            game.bot.hand.forEach(card => {
                const cardElement = createCardElement(card, false);
                elements.botCards.appendChild(cardElement);
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
                    <div class="card-value bottom">${card.value}</div>
                `;
            }
            
            return div;
        }
        
        // Обновление интерфейса
        function updateUI() {
            elements.playerBalance.textContent = game.player.balance;
            elements.botBalance.textContent = game.bot.balance;
            elements.pot.textContent = game.pot;
            elements.potValue.textContent = game.pot;
            elements.round.textContent = game.round;
            elements.currentBet.textContent = game.currentBet;
            
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
            
            updateCombinations();
        }
        
        // Установка статуса
        function setStatus(text) {
            elements.gameStatus.textContent = text;
        }
        
        // Добавление в лог
        function addLog(message) {
            const time = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            const entry = document.createElement('div');
            entry.className = 'log-entry';
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
    </script>
</body>
</html>
