
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Квиз "Год единства народов России"</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            touch-action: manipulation;
        }

        :root {
            --primary: #2c3e50;
            --secondary: #e74c3c;
            --accent: #3498db;
            --light: #ecf0f1;
            --dark: #2c3e50;
            --success: #27ae60;
            --warning: #f39c12;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }

        body {
            background: linear-gradient(135deg, #1a237e 0%, #311b92 100%);
            color: var(--light);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 15px;
            overflow-x: hidden;
        }

        .container {
            width: 100%;
            max-width: 800px;
            background-color: rgba(255, 255, 255, 0.98);
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            padding: 25px;
            color: var(--dark);
            position: relative;
            overflow: hidden;
            -webkit-overflow-scrolling: touch;
        }

        .container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 10px;
            background: linear-gradient(90deg, #e74c3c, #3498db, #27ae60, #f39c12);
        }

        h1, h2, h3 {
            text-align: center;
            margin-bottom: 20px;
            color: var(--primary);
        }

        h1 {
            font-size: clamp(1.8rem, 4vw, 2.5rem);
            background: linear-gradient(90deg, #e74c3c, #3498db);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
            margin-top: 10px;
            line-height: 1.2;
        }

        .subtitle {
            text-align: center;
            color: var(--secondary);
            font-size: clamp(1rem, 3vw, 1.2rem);
            margin-bottom: 25px;
            font-weight: 500;
            line-height: 1.4;
        }

        .screen {
            display: none;
            animation: fadeIn 0.8s ease;
        }

        .screen.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .form-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            margin-bottom: 10px;
            font-weight: 600;
            color: var(--primary);
            font-size: 1.1rem;
        }

        input[type="text"] {
            width: 100%;
            padding: 18px 20px;
            border: 2px solid #ddd;
            border-radius: 12px;
            font-size: 1.1rem;
            transition: var(--transition);
            background: white;
            min-height: 60px;
            font-size: 18px;
        }

        input[type="text"]:focus {
            border-color: var(--accent);
            outline: none;
            box-shadow: 0 0 0 3px rgba(52, 152, 219, 0.2);
        }

        .rules-list {
            background-color: #f8f9fa;
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
            border-left: 5px solid var(--accent);
        }

        .rules-list ul {
            list-style: none;
        }

        .rules-list li {
            margin-bottom: 18px;
            padding-left: 10px;
            line-height: 1.6;
            font-size: 1.1rem;
        }

        .rules-list i {
            color: var(--accent);
            margin-right: 12px;
            width: 24px;
            text-align: center;
        }

        .btn {
            display: block;
            width: 100%;
            padding: 22px 20px;
            background: linear-gradient(90deg, var(--secondary), #c0392b);
            color: white;
            border: none;
            border-radius: 12px;
            font-size: 1.2rem;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
            margin-top: 20px;
            text-align: center;
            text-decoration: none;
            box-shadow: var(--shadow);
            min-height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            touch-action: manipulation;
            user-select: none;
        }

        .btn:hover, .btn:active {
            transform: translateY(-3px);
            box-shadow: 0 7px 14px rgba(0, 0, 0, 0.2);
            background: linear-gradient(90deg, #c0392b, var(--secondary));
        }

        .btn:active {
            transform: translateY(1px);
        }

        .btn-secondary {
            background: linear-gradient(90deg, var(--accent), #2980b9);
        }

        .btn-secondary:hover, .btn-secondary:active {
            background: linear-gradient(90deg, #2980b9, var(--accent));
        }

        .btn-success {
            background: linear-gradient(90deg, var(--success), #219653);
        }

        .btn-success:hover, .btn-success:active {
            background: linear-gradient(90deg, #219653, var(--success));
        }

        .question-counter {
            display: flex;
            justify-content: space-between;
            margin-bottom: 25px;
            padding: 20px;
            background-color: #f8f9fa;
            border-radius: 12px;
            font-weight: 600;
            color: var(--primary);
            border: 2px solid #e9ecef;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }

        .timer {
            font-size: 1.8rem;
            color: var(--secondary);
            font-weight: 700;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .question {
            font-size: clamp(1.2rem, 3vw, 1.3rem);
            line-height: 1.6;
            margin-bottom: 30px;
            padding: 25px;
            background-color: #f8f9fa;
            border-radius: 12px;
            border-left: 5px solid var(--accent);
            min-height: 140px;
            display: flex;
            align-items: center;
        }

        .options {
            display: grid;
            grid-template-columns: 1fr;
            gap: 20px;
            margin-bottom: 30px;
        }

        @media (min-width: 768px) {
            .options {
                grid-template-columns: 1fr 1fr;
            }
        }

        .option {
            padding: 25px 20px;
            background-color: white;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: var(--transition);
            font-size: 1.1rem;
            position: relative;
            overflow: hidden;
            min-height: 90px;
            display: flex;
            align-items: center;
            user-select: none;
            -webkit-user-select: none;
            touch-action: manipulation;
        }

        .option:hover, .option:active {
            border-color: var(--accent);
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .option.selected {
            border-color: var(--accent);
            background-color: rgba(52, 152, 219, 0.1);
            transform: translateY(-3px);
        }

        .option.correct {
            border-color: var(--success);
            background-color: rgba(39, 174, 96, 0.1);
        }

        .option.incorrect {
            border-color: var(--secondary);
            background-color: rgba(231, 76, 60, 0.1);
        }

        .option-label {
            display: inline-flex;
            align-items: center;
            justify-content: center;
            width: 44px;
            height: 44px;
            background-color: var(--primary);
            color: white;
            border-radius: 50%;
            text-align: center;
            line-height: 44px;
            margin-right: 20px;
            font-weight: 600;
            font-size: 1.2rem;
            flex-shrink: 0;
        }

        .result-screen {
            text-align: center;
            padding: 30px 0;
        }

        .result-score {
            font-size: clamp(3rem, 10vw, 5rem);
            font-weight: 700;
            color: var(--secondary);
            margin: 30px 0;
            text-shadow: 3px 3px 0 rgba(0, 0, 0, 0.1);
        }

        .result-message {
            font-size: clamp(1.3rem, 4vw, 1.5rem);
            margin-bottom: 30px;
            color: var(--primary);
            line-height: 1.4;
        }

        .result-details {
            background-color: #f8f9fa;
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 30px;
            text-align: left;
        }

        .result-details h3 {
            text-align: center;
            margin-bottom: 25px;
            color: var(--accent);
        }

        .result-details p {
            margin-bottom: 18px;
            font-size: 1.1rem;
            line-height: 1.5;
        }

        .progress-bar {
            height: 12px;
            background-color: #e9ecef;
            border-radius: 6px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, var(--accent), var(--success));
            width: 0%;
            transition: width 0.5s ease;
        }

        .flag-decoration {
            display: flex;
            justify-content: center;
            margin-bottom: 30px;
            gap: 8px;
        }

        .flag-color {
            width: clamp(25px, 5vw, 30px);
            height: clamp(50px, 10vw, 60px);
            border-radius: 5px;
        }

        .flag-color:nth-child(1) { background-color: #e74c3c; }
        .flag-color:nth-child(2) { background-color: #3498db; }
        .flag-color:nth-child(3) { background-color: #f1c40f; }
        .flag-color:nth-child(4) { background-color: #27ae60; }

        .header {
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 25px;
            gap: 20px;
            flex-wrap: wrap;
        }

        .logo {
            font-size: clamp(2.5rem, 6vw, 3rem);
            color: var(--secondary);
        }

        .player-name {
            position: absolute;
            top: 20px;
            right: 30px;
            font-size: 1.1rem;
            color: var(--primary);
            font-weight: 600;
            background: rgba(255, 255, 255, 0.9);
            padding: 8px 15px;
            border-radius: 20px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }

        .confirmation-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 2000;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.3s ease, visibility 0.3s ease;
        }

        .confirmation-modal.active {
            opacity: 1;
            visibility: visible;
        }

        .confirmation-content {
            background-color: white;
            border-radius: 20px;
            padding: 30px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            text-align: center;
            transform: translateY(-20px);
            transition: transform 0.3s ease;
        }

        .confirmation-modal.active .confirmation-content {
            transform: translateY(0);
        }

        .confirmation-content h3 {
            color: var(--primary);
            margin-bottom: 20px;
            font-size: 1.5rem;
        }

        .confirmation-text {
            font-size: 1.2rem;
            margin-bottom: 30px;
            color: var(--dark);
            line-height: 1.5;
        }

        .confirmation-buttons {
            display: flex;
            gap: 15px;
        }

        .confirmation-buttons button {
            flex: 1;
            padding: 18px 20px;
            border: none;
            border-radius: 12px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: var(--transition);
        }

        .confirm-yes {
            background: linear-gradient(90deg, var(--success), #219653);
            color: white;
        }

        .confirm-no {
            background: linear-gradient(90deg, #95a5a6, #7f8c8d);
            color: white;
        }

        @media (max-width: 768px) {
            .container {
                padding: 20px;
            }
            
            .player-name {
                position: relative;
                top: 0;
                right: 0;
                text-align: center;
                margin-bottom: 20px;
                display: inline-block;
                margin-left: auto;
                margin-right: auto;
            }
            
            .question {
                padding: 20px;
                font-size: 1.2rem;
            }
            
            .option {
                padding: 22px 18px;
            }
            
            .option-label {
                width: 40px;
                height: 40px;
                line-height: 40px;
                font-size: 1.1rem;
            }
            
            .btn {
                padding: 20px;
                min-height: 65px;
            }
            
            .confirmation-content {
                padding: 25px;
            }
            
            .confirmation-buttons {
                flex-direction: column;
            }
        }

        @media (max-width: 480px) {
            body {
                padding: 10px;
            }
            
            .container {
                padding: 18px;
                border-radius: 16px;
            }
            
            .question-counter {
                padding: 15px;
                flex-direction: column;
                align-items: stretch;
                text-align: center;
            }
            
            .timer {
                font-size: 1.6rem;
                justify-content: center;
            }
            
            .option {
                padding: 20px 15px;
                min-height: 80px;
            }
            
            .option-label {
                width: 36px;
                height: 36px;
                line-height: 36px;
                margin-right: 15px;
            }
        }

        .pulse {
            animation: pulse 1.5s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        .shake {
            animation: shake 0.5s;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-5px); }
            20%, 40%, 60%, 80% { transform: translateX(5px); }
        }

        .confetti {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #f00;
            opacity: 0;
            pointer-events: none;
            z-index: 1000;
        }

        .timeout-message {
            background-color: rgba(231, 76, 60, 0.1);
            border: 2px solid var(--secondary);
            border-radius: 12px;
            padding: 20px;
            margin-bottom: 25px;
            text-align: center;
            color: var(--secondary);
            font-weight: 600;
            font-size: 1.2rem;
            display: none;
        }

        .timeout-message.show {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        .thank-you-message {
            background-color: rgba(39, 174, 96, 0.1);
            border: 2px solid var(--success);
            border-radius: 12px;
            padding: 25px;
            margin: 30px 0;
            text-align: center;
            color: var(--primary);
            font-size: 1.3rem;
            line-height: 1.6;
        }

        .share-section {
            background-color: #f8f9fa;
            border-radius: 12px;
            padding: 25px;
            margin-top: 30px;
            text-align: center;
        }

        .share-section h3 {
            color: var(--accent);
            margin-bottom: 20px;
        }

        .telegram-btn {
            display: inline-flex;
            align-items: center;
            gap: 10px;
            background: linear-gradient(90deg, #0088cc, #006699);
            color: white;
            padding: 18px 30px;
            border-radius: 12px;
            text-decoration: none;
            font-weight: 600;
            font-size: 1.1rem;
            transition: var(--transition);
            margin-top: 15px;
            min-height: 60px;
        }

        .telegram-btn:hover, .telegram-btn:active {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            background: linear-gradient(90deg, #006699, #0088cc);
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="flag-decoration">
            <div class="flag-color"></div>
            <div class="flag-color"></div>
            <div class="flag-color"></div>
            <div class="flag-color"></div>
        </div>
        
        <div id="start-screen" class="screen active">
            <div class="header">
                <i class="fas fa-landmark logo"></i>
                <div>
                    <h1>Год единства народов России</h1>
                    <div class="subtitle">Заочный институт против СТФ: битва умов</div>
                </div>
            </div>
            
            <div class="form-group">
                <label for="player-name"><i class="fas fa-user"></i> Введите ваше имя:</label>
                <input type="text" id="player-name" placeholder="Иван Иванов" maxlength="30">
            </div>
            
            <button id="start-btn" class="btn pulse">
                <i class="fas fa-play-circle"></i> Начать игру
            </button>
        </div>
        
        <div id="rules-screen" class="screen">
            <h2><i class="fas fa-scroll"></i> Правила квиза</h2>
            <div class="rules-list">
                <ul>
                    <li><i class="fas fa-clock"></i> На каждый вопрос даётся <strong>120 секунд</strong></li>
                    <li><i class="fas fa-check-circle"></i> За верный ответ вы получаете <strong>1 балл</strong></li>
                    <li><i class="fas fa-list-ol"></i> Всего в квизе <strong>30 сложных расчётных задач</strong> для студентов строительных специальностей</li>
                    <li><i class="fas fa-brain"></i> Задачи связаны с тематикой <strong>единства народов России</strong></li>
                    <li><i class="fas fa-mobile-alt"></i> Игра оптимизирована для мобильных устройств</li>
                    <li><i class="fas fa-trophy"></i> Постарайтесь набрать как можно больше баллов!</li>
                </ul>
            </div>
            
            <button id="rules-back-btn" class="btn btn-secondary">
                <i class="fas fa-arrow-left"></i> Назад
            </button>
            
            <button id="start-game-btn" class="btn">
                <i class="fas fa-play"></i> Начать квиз
            </button>
        </div>
        
        <div id="game-screen" class="screen">
            <div class="player-name" id="current-player">Игрок: </div>
            
            <div class="question-counter">
                <div class="current-question">Вопрос <span id="question-number">1</span> из 30</div>
                <div class="timer">
                    <i class="fas fa-clock"></i> <span id="timer">120</span> сек
                </div>
            </div>
            
            <div class="progress-bar">
                <div class="progress" id="progress-bar"></div>
            </div>
            
            <div class="timeout-message" id="timeout-message">
                <i class="fas fa-exclamation-triangle"></i> Время вышло! Ответ не засчитан.
            </div>
            
            <div class="question" id="question-text">
                В регионе проживают русские (56 %), татары (22 %) и ещё 3 народа с равной долей. Если общая численность — 2 млн человек, сколько представителей каждого из трёх малых народов?
            </div>
            
            <div class="mobile-tap-hint">
                <i class="fas fa-hand-point-up"></i> Нажмите на вариант ответа для выбора
            </div>
            
            <div class="options" id="options-container">
            </div>
            
            <button id="next-btn" class="btn" disabled>
                <i class="fas fa-forward"></i> Следующий вопрос
            </button>
        </div>
        
        <div id="result-screen" class="screen">
            <div class="result-screen">
                <h2><i class="fas fa-trophy"></i> Результаты квиза</h2>
                <div class="result-score" id="final-score">0</div>
                <div class="result-message" id="result-message"></div>
                
                <div class="thank-you-message">
                    <i class="fas fa-heart" style="color: #e74c3c; font-size: 2rem; margin-bottom: 15px;"></i>
                    <h3>Благодарим за участие в квизе!</h3>
                    <p>Вы проявили отличные знания в области математических расчетов и тематики единства народов России.</p>
                    <p>Ваш результат — это доказательство ваших способностей!</p>
                </div>
                
                <div class="result-details">
                    <h3><i class="fas fa-chart-line"></i> Статистика</h3>
                    <p><strong>Игрок:</strong> <span id="result-player"></span></p>
                    <p><strong>Правильных ответов:</strong> <span id="correct-answers">0</span> из 30</p>
                    <p><strong>Время прохождения:</strong> <span id="total-time">0</span> сек</p>
                    <p><strong>Дата прохождения:</strong> <span id="completion-date"></span></p>
                </div>
                
                <div class="share-section">
                    <h3><i class="fas fa-share-alt"></i> Отправить результат</h3>
                    <a href="https://t.me/seshhollowbones" class="telegram-btn" target="_blank">
                        <i class="fab fa-telegram"></i> Отправить результаты в Telegram
                    </a>
                </div>
            </div>
        </div>
    </div>

    <div class="confirmation-modal" id="confirmation-modal">
        <div class="confirmation-content">
            <h3><i class="fas fa-question-circle"></i> Подтверждение ответа</h3>
            <div class="confirmation-text" id="confirmation-text">
                Вы выбрали вариант ответа. Вы уверены, что хотите подтвердить этот выбор?
            </div>
            <div class="confirmation-buttons">
                <button class="confirm-yes" id="confirm-yes">
                    <i class="fas fa-check"></i> Да, подтверждаю
                </button>
                <button class="confirm-no" id="confirm-no">
                    <i class="fas fa-times"></i> Нет, выбрать другой
                </button>
            </div>
        </div>
    </div>

    <script>
        const quizData = [
            {
                question: "В регионе проживают русские (56 %), татары (22 %) и ещё 3 народа с равной долей. Если общая численность — 2 млн человек, сколько представителей каждого из трёх малых народов?",
                options: ["120 тыс.", "146 667", "160 тыс.", "180 тыс."],
                correct: 1
            },
            {
                question: "На фестивале народного творчества участвуют 4 коллектива: русские, башкиры, чуваши и мордва. Русские составляют 45 % участников, башкиры — ⅔ от числа русских, чуваши — на 20 человек меньше башкир. Если всего 300 человек, сколько мордвы?",
                options: ["35", "40", "45", "50"],
                correct: 0
            },
            {
                question: "Площадь многонационального округа — 850 км². Леса занимают 36 %, пашни — 42 %. Остальная территория — поселения и дороги (в соотношении 3 : 1). Какова площадь дорог?",
                options: ["42,5 км²", "66 км²", "51 км²", "68 км²"],
                correct: 0
            },
            {
                question: "В строительстве объекта работали 150 рабочих: русские, татары, удмурты. Русских на 30 больше, чем татар, а удмуртов — в 1,5 раза меньше, чем татар. Сколько удмуртов?",
                options: ["20", "24", "30", "36"],
                correct: 1
            },
            {
                question: "Длина моста — 600 м. За первый месяц построили ⅜ длины, за второй — 40 % остатка, за третий — ½ нового остатка. Сколько метров осталось?",
                options: ["135 м", "150 м", "165 м", "180 м"],
                correct: 0
            },
            {
                question: "Население города за 6 лет выросло на 22 %, достигнув 488 тыс. Какова была численность 6 лет назад?",
                options: ["390 тыс.", "400 тыс.", "410 тыс.", "420 тыс."],
                correct: 1
            },
            {
                question: "Для укладки плитки закупили 3 000 штук. В первый день израсходовали 40 %, во второй — ⅗ остатка, в третий — 75 % нового остатка. Сколько плиток осталось?",
                options: ["180", "216", "240", "270"],
                correct: 0
            },
            {
                question: "Высота башни — 144 м. На чертеже в масштабе 1 : 75 её высота 1,8 см. Соответствует ли чертёж масштабу?",
                options: ["да, точно", "нет, занижена на 0,2 см", "нет, завышена на 0,3 см", "нет, занижена на 0,4 см"],
                correct: 3
            },
            {
                question: "В команде строителей 96 человек. Русские составляют ⁵⁄₁₂ команды, татары — ⅔ русских, остальные — представители других народов. Сколько «других»?",
                options: ["16", "20", "24", "28"],
                correct: 0
            },
            {
                question: "Смета на строительство: материалы — 2,1 млн руб., работа — 1,35 млн руб. Накладные расходы — 22 % от суммы материалов и работы. Итоговая стоимость с НДС (20 %)?",
                options: ["4 536 тыс. руб.", "4 622,4 тыс. руб.", "4 704 тыс. руб.", "4 800 тыс. руб."],
                correct: 1
            },
            {
                question: "В районе 300 сёл. Газифицировано 48 % из них. Из оставшихся ⅖ имеют доступ к газу через соседей. Сколько сёл полностью без газа?",
                options: ["126", "132", "144", "156"],
                correct: 1
            },
            {
                question: "Длина дороги — 240 км. За первый месяц построили 35 %, за второй — на 30 % больше, чем за первый. Сколько км осталось?",
                options: ["72 км", "84 км", "96 км", "108 км"],
                correct: 0
            },
            {
                question: "В школе 1 200 учеников. Дети мигрантов — 28 %. Из них ⅜ — из стран СНГ, остальные — из Средней Азии. Сколько учеников из Средней Азии?",
                options: ["210", "252", "280", "336"],
                correct: 0
            },
            {
                question: "Плотность бетона — 2 400 кг/м³. Для фундамента нужно 7,2 м³ раствора. Масса с учётом 6 % потерь при заливке?",
                options: ["17 280 кг", "18 316,8 кг", "19 008 кг", "19 440 кг"],
                correct: 1
            },
            {
                question: "В конкурсе участвовали 200 человек: русские, татары, башкиры, марийцы. Русские — 45 %, татары — на 25 человек меньше русских, башкиры — ½ татар. Сколько марийцев?",
                options: ["10", "15", "20", "25"],
                correct: 2
            },
            {
                question: "Площадь участка — 2,5 га. Под застройку отведено 64 %. Из остатка 20 % займёт парковка, 30 % — сквер, остальное — дорожки. Какова площадь дорожек?",
                options: ["0,36 га", "0,45 га", "0,50 га", "0,60 га"],
                correct: 0
            },
            {
                question: "Высота здания — 86,4 м. Первые 6 этажей — по 3,6 м, остальные — по 3,2 м. Сколько всего этажей?",
                options: ["22", "23", "24", "25"],
                correct: 2
            },
            {
                question: "В регионе 500 тыс. жителей. За 7 лет 14 % переехали, но прибыло 10 % новых жителей. Какова численность сейчас?",
                options: ["472 тыс.", "482 тыс.", "490 тыс.", "504 тыс."],
                correct: 1
            },
            {
                question: "Для фундамента нужно 12 м³ щебня. Грузовик везёт 2,4 м³ за рейс, но 8 % груза теряется при погрузке. Сколько рейсов потребуется?",
                options: ["5", "6", "7", "8"],
                correct: 1
            },
            {
                question: "В селе 240 домов. Отремонтировано 70 %, но 25 % отремонтированных требуют доработки. Сколько домов полностью готовы?",
                options: ["126", "140", "154", "168"],
                correct: 0
            },
            {
                question: "В многонациональном посёлке 450 жителей: русские, татары, башкиры и удмурты. Русские составляют 40 %, татары — ⅔ от числа русских, башкиры — на 15 человек меньше татар. Сколько удмуртов?",
                options: ["45", "60", "75", "90"],
                correct: 2
            },
            {
                question: "На межрегиональной стройке работают 240 человек. Русские — 55 %, татары — ⅖ от числа русских, остальные — представители других народов. Сколько «других»?",
                options: ["36", "48", "60", "72"],
                correct: 3
            },
            {
                question: "Площадь культурного комплекса — 1,8 га. Под здания отведено 55 %, под парки — ⅓ остатка. Остальная территория — пешеходные зоны. Какова их площадь?",
                options: ["0,45 га", "0,54 га", "0,60 га", "0,72 га"],
                correct: 1
            },
            {
                question: "Высота минарета мечети — 54 м, колокольни храма — 48 м. На чертеже в масштабе 1 : 60 разница их высот составляет 1 см. Соответствует ли чертёж масштабу?",
                options: ["да, точно", "нет, занижена на 0,1 см", "нет, завышена на 0,2 см", "нет, занижена на 0,3 см"],
                correct: 3
            },
            {
                question: "В регионе 420 сёл. Газифицировано 52 % из них. Из оставшихся ⅜ имеют доступ к газу через соседей. Сколько сёл полностью без газа?",
                options: ["147", "168", "189", "210"],
                correct: 0
            },
            {
                question: "Длина автодороги — 360 км. За первый месяц построили 40 %, за второй — на 20 % больше, чем за первый. Сколько км осталось?",
                options: ["72 км", "86,4 км", "96 км", "108 км"],
                correct: 1
            },
            {
                question: "В школе 1 500 учеников. Дети мигрантов — 24 %. Из них ⅝ — из стран СНГ, остальные — из Закавказья. Сколько учеников из Закавказья?",
                options: ["180", "216", "240", "288"],
                correct: 1
            },
            {
                question: "Плотность кирпича — 1 800 кг/м³. Для кладки стены нужно 4,5 м³ материала. Масса с учётом 7 % потерь при укладке?",
                options: ["7 560 кг", "8 127 кг", "8 460 кг", "8 820 кг"],
                correct: 1
            },
            {
                question: "В конкурсе участвовали 250 человек: русские, татары, чуваши, марийцы. Русские — 48 %, татары — на 30 человек меньше русских, чуваши — ½ татар. Сколько марийцев?",
                options: ["10", "15", "20", "25"],
                correct: 2
            },
            {
                question: "Площадь участка — 3,2 га. Под застройку отведено 60 %. Из остатка 25 % займёт сквер, 40 % — спортивная зона, остальное — велодорожки. Какова площадь велодорожек?",
                options: ["0,448 га", "0,560 га", "0,640 га", "0,768 га"],
                correct: 0
            }
        ];

        const screens = {
            start: document.getElementById('start-screen'),
            rules: document.getElementById('rules-screen'),
            game: document.getElementById('game-screen'),
            result: document.getElementById('result-screen')
        };
        
        const playerNameInput = document.getElementById('player-name');
        const currentPlayerElement = document.getElementById('current-player');
        const resultPlayerElement = document.getElementById('result-player');
        
        const questionNumberElement = document.getElementById('question-number');
        const questionTextElement = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const progressBarElement = document.getElementById('progress-bar');
        const timeoutMessage = document.getElementById('timeout-message');
        
        const timerElement = document.getElementById('timer');
        const nextButton = document.getElementById('next-btn');
        
        const finalScoreElement = document.getElementById('final-score');
        const correctAnswersElement = document.getElementById('correct-answers');
        const totalTimeElement = document.getElementById('total-time');
        const completionDateElement = document.getElementById('completion-date');
        const resultMessageElement = document.getElementById('result-message');
        
        const startButton = document.getElementById('start-btn');
        const rulesBackButton = document.getElementById('rules-back-btn');
        const startGameButton = document.getElementById('start-game-btn');
        
        const confirmationModal = document.getElementById('confirmation-modal');
        const confirmationText = document.getElementById('confirmation-text');
        const confirmYesButton = document.getElementById('confirm-yes');
        const confirmNoButton = document.getElementById('confirm-no');
        
        let currentQuestionIndex = 0;
        let score = 0;
        let playerName = '';
        let timeLeft = 120;
        let timerInterval = null;
        let startTime = 0;
        let selectedOption = null;
        let answerSubmitted = false;
        let totalGameTime = 0;
        let questionStartTime = 0;
        let pendingOptionElement = null;
        let pendingOptionIndex = null;
        
        function initGame() {
            currentQuestionIndex = 0;
            score = 0;
            timeLeft = 120;
            selectedOption = null;
            answerSubmitted = false;
            totalGameTime = 0;
            pendingOptionElement = null;
            pendingOptionIndex = null;
            
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            
            timeoutMessage.classList.remove('show');
            
            loadQuestion();
            
            currentPlayerElement.textContent = `Игрок: ${playerName}`;
        }
        
        function loadQuestion() {
            const question = quizData[currentQuestionIndex];
            
            questionNumberElement.textContent = currentQuestionIndex + 1;
            
            questionTextElement.textContent = question.question;
            
            progressBarElement.style.width = `${((currentQuestionIndex + 1) / quizData.length) * 100}%`;
            
            optionsContainer.innerHTML = '';
            
            const optionLabels = ['а', 'б', 'в', 'г'];
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.classList.add('option');
                optionElement.dataset.index = index;
                
                optionElement.innerHTML = `
                    <span class="option-label">${optionLabels[index]}</span>
                    <span class="option-text">${option}</span>
                `;
                
                optionElement.addEventListener('click', () => showConfirmation(optionElement, index));
                optionElement.addEventListener('touchstart', (e) => {
                    e.preventDefault();
                    showConfirmation(optionElement, index);
                }, { passive: false });
                
                optionsContainer.appendChild(optionElement);
            });
            
            nextButton.disabled = true;
            answerSubmitted = false;
            selectedOption = null;
            pendingOptionElement = null;
            pendingOptionIndex = null;
            
            startTimerForQuestion();
        }
        
        function showConfirmation(optionElement, optionIndex) {
            if (answerSubmitted) return;
            
            pendingOptionElement = optionElement;
            pendingOptionIndex = optionIndex;
            
            const optionLabels = ['а', 'б', 'в', 'г'];
            const optionText = optionElement.querySelector('.option-text').textContent;
            
            confirmationText.innerHTML = `Вы выбрали вариант <strong>${optionLabels[optionIndex]}) ${optionText}</strong>.<br>Вы уверены, что хотите подтвердить этот выбор?`;
            
            confirmationModal.classList.add('active');
            document.body.style.overflow = 'hidden';
        }
        
        function hideConfirmation() {
            confirmationModal.classList.remove('active');
            document.body.style.overflow = 'auto';
        }
        
        function confirmSelection() {
            if (pendingOptionElement && pendingOptionIndex !== null) {
                document.querySelectorAll('.option').forEach(opt => {
                    opt.classList.remove('selected');
                });
                
                pendingOptionElement.classList.add('selected');
                selectedOption = pendingOptionIndex;
                
                pendingOptionElement.classList.add('pulse');
                setTimeout(() => {
                    pendingOptionElement.classList.remove('pulse');
                }, 300);
                
                nextButton.disabled = false;
                
                setTimeout(() => {
                    if (!answerSubmitted && selectedOption !== null) {
                        checkAnswer();
                        nextButton.disabled = false;
                    }
                }, 1000);
            }
            
            hideConfirmation();
        }
        
        function cancelSelection() {
            if (pendingOptionElement) {
                pendingOptionElement.classList.remove('selected');
            }
            pendingOptionElement = null;
            pendingOptionIndex = null;
            selectedOption = null;
            hideConfirmation();
        }
        
        function startTimerForQuestion() {
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            
            timeLeft = 120;
            timerElement.textContent = timeLeft;
            timerElement.style.color = '#e74c3c';
            timerElement.classList.remove('pulse');
            
            questionStartTime = Date.now();
            
            timerInterval = setInterval(() => {
                timeLeft--;
                timerElement.textContent = timeLeft;
                
                if (timeLeft <= 30) {
                    timerElement.style.color = '#e74c3c';
                    
                    if (timeLeft <= 10) {
                        timerElement.classList.add('pulse');
                    }
                }
                
                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    handleTimeout();
                }
            }, 1000);
        }
        
        function checkAnswer() {
            if (selectedOption === null) return;
            
            answerSubmitted = true;
            
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            
            const question = quizData[currentQuestionIndex];
            const options = document.querySelectorAll('.option');
            
            options.forEach(opt => {
                opt.style.cursor = 'default';
                opt.style.pointerEvents = 'none';
            });
            
            options.forEach((opt, index) => {
                if (index === question.correct) {
                    opt.classList.add('correct');
                } else if (index === selectedOption && index !== question.correct) {
                    opt.classList.add('incorrect');
                    opt.classList.add('shake');
                }
            });
            
            if (selectedOption === question.correct) {
                score++;
                
                options[question.correct].classList.add('pulse');
                
                createConfetti();
            }
        }
        
        function handleTimeout() {
            if (answerSubmitted) return;
            
            timeoutMessage.classList.add('show');
            
            document.querySelectorAll('.option').forEach(opt => {
                opt.style.pointerEvents = 'none';
            });
            
            setTimeout(() => {
                timeoutMessage.classList.remove('show');
                nextQuestion();
            }, 3000);
        }
        
        function nextQuestion() {
            if (!answerSubmitted && selectedOption === null) {
            }
            
            if (!answerSubmitted && selectedOption !== null) {
                checkAnswer();
                return;
            }
            
            currentQuestionIndex++;
            
            if (currentQuestionIndex < quizData.length) {
                loadQuestion();
            } else {
                finishGame();
            }
        }
        
        function finishGame() {
            if (timerInterval) {
                clearInterval(timerInterval);
            }
            
            totalGameTime = Math.floor((Date.now() - startTime) / 1000);
            
            const percentage = Math.round((score / quizData.length) * 100);
            
            finalScoreElement.textContent = score;
            correctAnswersElement.textContent = `${score} из ${quizData.length}`;
            totalTimeElement.textContent = totalGameTime;
            resultPlayerElement.textContent = playerName;
            
            const now = new Date();
            completionDateElement.textContent = now.toLocaleDateString('ru-RU', {
                day: 'numeric',
                month: 'long',
                year: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
            
            let message = "";
            if (percentage >= 90) {
                message = "🏆 Отличный результат! Вы настоящий эксперт по теме единства народов России!";
            } else if (percentage >= 70) {
                message = "👍 Хороший результат! Вы хорошо разбираетесь в теме!";
            } else if (percentage >= 50) {
                message = "👌 Неплохой результат! Есть куда стремиться!";
            } else {
                message = "💪 Попробуйте ещё раз! Вы сможете лучше!";
            }
            
            resultMessageElement.textContent = message;
            
            showScreen('result');
        }
        
        function showScreen(screenName) {
            Object.values(screens).forEach(screen => {
                screen.classList.remove('active');
            });
            
            screens[screenName].classList.add('active');
            
            if (screenName === 'game') {
                startTime = Date.now();
            }
        }
        
        function createConfetti() {
            const colors = ['#e74c3c', '#3498db', '#f1c40f', '#27ae60', '#9b59b6'];
            const container = document.querySelector('.container');
            
            for (let i = 0; i < 20; i++) {
                const confetti = document.createElement('div');
                confetti.classList.add('confetti');
                
                const size = Math.random() * 10 + 5;
                const color = colors[Math.floor(Math.random() * colors.length)];
                const left = Math.random() * 100;
                const animationDuration = Math.random() * 3 + 2;
                
                confetti.style.width = `${size}px`;
                confetti.style.height = `${size}px`;
                confetti.style.backgroundColor = color;
                confetti.style.left = `${left}%`;
                confetti.style.opacity = '1';
                confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
                
                confetti.style.animation = `
                    confettiFall ${animationDuration}s linear forwards,
                    confettiRotate ${animationDuration/2}s linear infinite
                `;
                
                container.appendChild(confetti);
                
                setTimeout(() => {
                    if (confetti.parentNode) {
                        confetti.remove();
                    }
                }, animationDuration * 1000);
            }
        }
        
        startButton.addEventListener('click', () => {
            playerName = playerNameInput.value.trim();
            
            if (!playerName) {
                playerName = 'Игрок';
                playerNameInput.value = playerName;
            }
            
            showScreen('rules');
        });
        
        startButton.addEventListener('touchstart', (e) => {
            e.preventDefault();
            startButton.click();
        }, { passive: false });
        
        rulesBackButton.addEventListener('click', () => {
            showScreen('start');
        });
        
        rulesBackButton.addEventListener('touchstart', (e) => {
            e.preventDefault();
            rulesBackButton.click();
        }, { passive: false });
        
        startGameButton.addEventListener('click', () => {
            showScreen('game');
            initGame();
        });
        
        startGameButton.addEventListener('touchstart', (e) => {
            e.preventDefault();
            startGameButton.click();
        }, { passive: false });
        
        nextButton.addEventListener('click', () => {
            nextQuestion();
        });
        
        nextButton.addEventListener('touchstart', (e) => {
            e.preventDefault();
            nextButton.click();
        }, { passive: false });
        
        confirmYesButton.addEventListener('click', () => {
            confirmSelection();
        });
        
        confirmNoButton.addEventListener('click', () => {
            cancelSelection();
        });
        
        confirmationModal.addEventListener('click', (e) => {
            if (e.target === confirmationModal) {
                cancelSelection();
            }
        });
        
        document.addEventListener('keydown', (e) => {
            if (confirmationModal.classList.contains('active')) {
                if (e.key === 'Enter') {
                    confirmSelection();
                } else if (e.key === 'Escape') {
                    cancelSelection();
                }
                return;
            }
            
            if (!answerSubmitted && e.key >= '1' && e.key <= '4') {
                const optionIndex = parseInt(e.key) - 1;
                const optionElement = document.querySelectorAll('.option')[optionIndex];
                
                if (optionElement) {
                    showConfirmation(optionElement, optionIndex);
                }
            }
            
            if (e.key === 'Enter' && selectedOption !== null && !answerSubmitted) {
                checkAnswer();
                nextButton.disabled = false;
            }
            
            if (e.key === ' ' && answerSubmitted) {
                nextQuestion();
            }
        });
        
        window.addEventListener('DOMContentLoaded', () => {
            playerNameInput.focus();
            
            playerNameInput.placeholder = "Введите ваше имя";
            
            setInterval(() => {
                startButton.classList.toggle('pulse');
            }, 2000);
            
            const style = document.createElement('style');
            style.textContent = `
                @keyframes confettiFall {
                    0% { top: -20px; transform: rotate(0deg); opacity: 1; }
                    100% { top: 100%; transform: rotate(720deg); opacity: 0; }
                }
                
                @keyframes confettiRotate {
                    0% { transform: rotate(0deg); }
                    100% { transform: rotate(360deg); }
                }
            `;
            document.head.appendChild(style);
            
            document.addEventListener('touchstart', function() {}, {passive: true});
        });
    </script>
</body>
</html>
