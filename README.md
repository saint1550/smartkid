<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart Kid - Математический тренажёр</title>
    <link href="https://fonts.googleapis.com/css2?family=Comic+Neue:wght@400;700&display=swap" rel="stylesheet">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Comic Neue', 'Comic Sans MS', 'Marker Felt', 'Arial Rounded MT Bold', 
                         -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1c3a5c 0%, #6ab2e3 100%);
            min-height: 100vh;
            padding: 20px;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        .header {
            background: #1c3a5c;
            color: #f8f8f8;
            padding: 20px;
            text-align: center;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.3);
            border: 5px solid #6ab2e3;
        }

        .header h1 {
            font-size: clamp(2em, 5vw, 3em);
            text-shadow: 3px 3px 0 #6ab2e3;
            margin-bottom: 10px;
            color: #ffffff;
            font-weight: 700;
        }

        .header p {
            font-size: clamp(1em, 3vw, 1.2em);
            color: #e0f0ff;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            position: relative;
            min-height: 600px;
        }

        .grade-selection, .topic-selection, .content-area, .settings, .chat-container {
            background: #f8f8f8;
            padding: 30px;
            border-radius: 20px;
            margin-bottom: 30px;
            box-shadow: 0 8px 16px rgba(0,0,0,0.2);
            border: 4px solid #6ab2e3;
            position: relative;
            z-index: 10;
        }

        /* Стили для полноразмерных персонажей */
        .character-fullsize {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 250px;
            height: 350px;
            z-index: 5;
            pointer-events: none;
            transition: all 0.5s ease;
            transform-origin: bottom center;
        }

        .character-model {
            width: 100%;
            height: 100%;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: bottom center;
            position: absolute;
            bottom: 0;
            animation: float 3s ease-in-out infinite;
            filter: drop-shadow(0 10px 15px rgba(0,0,0,0.3));
        }

        .robot-model {
            background-image: url('https://cdn-icons-png.flaticon.com/512/3163/3163478.png');
        }

        .princess-model {
            background-image: url('https://cdn-icons-png.flaticon.com/512/4322/4322991.png');
        }

        .character-bubble {
            position: absolute;
            top: -80px;
            left: 50%;
            transform: translateX(-50%);
            background: #6ab2e3;
            color: #1c3a5c;
            padding: 15px;
            border-radius: 20px;
            border: 3px solid #1c3a5c;
            min-width: 200px;
            max-width: 250px;
            font-weight: 700;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            animation: bubbleFloat 2s ease-in-out infinite;
            display: none;
        }

        .character-bubble:after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            border-width: 15px 15px 0;
            border-style: solid;
            border-color: #6ab2e3 transparent transparent;
        }

        .character-bubble:before {
            content: '';
            position: absolute;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
            border-width: 17px 17px 0;
            border-style: solid;
            border-color: #1c3a5c transparent transparent;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-15px) rotate(2deg); }
        }

        @keyframes bubbleFloat {
            0%, 100% { transform: translateX(-50%) translateY(0px); }
            50% { transform: translateX(-50%) translateY(-5px); }
        }

        .grade-buttons {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }

        .grade-btn {
            background: #1c3a5c;
            color: #ffffff;
            border: none;
            padding: 25px 15px;
            font-size: clamp(1.2em, 3vw, 1.5em);
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 3px solid #6ab2e3;
            font-weight: 700;
            position: relative;
            z-index: 10;
        }

        .grade-btn:hover {
            background: #6ab2e3;
            transform: scale(1.05);
            color: #1c3a5c;
        }

        .topics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 15px;
            margin-top: 20px;
        }

        .topic-card {
            background: #6ab2e3;
            padding: 15px;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 3px solid #1c3a5c;
            text-align: center;
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            position: relative;
            z-index: 10;
        }

        .topic-card:hover {
            background: #1c3a5c;
            color: white;
            transform: scale(1.03);
        }

        .topic-card h3 {
            margin-bottom: 10px;
            font-size: clamp(1em, 2.5vw, 1.1em);
            font-weight: 700;
        }

        .topic-card p {
            font-size: clamp(0.8em, 2vw, 0.9em);
            opacity: 0.9;
        }

        .character-selection {
            display: flex;
            justify-content: center;
            gap: 40px;
            margin: 20px 0;
            flex-wrap: wrap;
        }

        .character-option {
            text-align: center;
            cursor: pointer;
            padding: 15px;
            border-radius: 15px;
            transition: all 0.3s ease;
            position: relative;
            z-index: 10;
        }

        .character-option:hover {
            background: #6ab2e3;
            transform: scale(1.1);
        }

        .character-option.selected {
            background: #1c3a5c;
            color: white;
            transform: scale(1.05);
        }

        .character-img {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            object-fit: cover;
            border: 4px solid #6ab2e3;
            transition: all 0.3s ease;
        }

        .character-option:hover .character-img {
            transform: rotate(10deg);
        }

        .theory-content, .practice-content {
            padding: 20px;
            background: #e8f4ff;
            border-radius: 15px;
            margin: 20px 0;
            border: 3px solid #1c3a5c;
            line-height: 1.6;
            position: relative;
            z-index: 10;
            min-height: 300px;
        }

        .theory-content {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .theory-text {
            flex: 1;
            padding: 20px;
            background: #6ab2e3;
            border-radius: 15px;
            color: #1c3a5c;
            font-size: 1.1em;
            line-height: 1.6;
            border: 3px solid #1c3a5c;
        }

        .task {
            background: white;
            padding: 20px;
            margin: 15px 0;
            border-radius: 15px;
            border: 2px solid #6ab2e3;
            position: relative;
            z-index: 10;
        }

        .task h4 {
            font-size: 1.2em;
            margin-bottom: 10px;
            font-weight: 700;
            color: #1c3a5c;
        }

        .task p {
            font-size: 1.1em;
            margin-bottom: 15px;
        }

        .task input {
            padding: 12px;
            margin: 10px 0;
            border: 2px solid #1c3a5c;
            border-radius: 10px;
            font-size: 1.1em;
            width: min(200px, 100%);
        }

        .task button {
            background: #1c3a5c;
            color: white;
            border: none;
            padding: 12px 24px;
            border-radius: 10px;
            cursor: pointer;
            margin-left: 10px;
            border: 2px solid #6ab2e3;
            font-weight: 700;
        }

        .task button:hover {
            background: #6ab2e3;
            color: #1c3a5c;
        }

        .result {
            margin-top: 10px;
            font-weight: bold;
            padding: 10px;
            border-radius: 10px;
            font-size: 1em;
        }

        .correct {
            background: #6ab2e3;
            color: white;
        }

        .incorrect {
            background: #1c3a5c;
            color: white;
        }

        .score-display {
            background: #1c3a5c;
            color: white;
            padding: 15px;
            border-radius: 15px;
            text-align: center;
            font-size: clamp(1em, 3vw, 1.2em);
            margin: 20px 0;
            border: 3px solid #6ab2e3;
            font-weight: 700;
            position: relative;
            z-index: 10;
        }

        .chat-container {
            position: fixed;
            bottom: 20px;
            right: 350px;
            width: min(350px, 90vw);
            height: min(500px, 80vh);
            display: none;
            flex-direction: column;
            border: 4px solid #1c3a5c;
            z-index: 1000;
        }

        .chat-header {
            background: #1c3a5c;
            color: white;
            padding: 15px;
            border-radius: 15px 15px 0 0;
            text-align: center;
            font-weight: 700;
        }

        .chat-messages {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            background: #f0f8ff;
        }

        .chat-input {
            display: flex;
            padding: 15px;
            background: white;
            border-radius: 0 0 15px 15px;
        }

        .chat-input input {
            flex: 1;
            padding: 12px;
            border: 2px solid #6ab2e3;
            border-radius: 10px;
            margin-right: 10px;
            font-size: 1em;
        }

        .chat-input button {
            background: #1c3a5c;
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: 10px;
            cursor: pointer;
            border: 2px solid #6ab2e3;
            font-weight: 700;
        }

        .chat-toggle {
            position: fixed;
            bottom: 20px;
            right: 350px;
            background: #1c3a5c;
            color: white;
            border: none;
            padding: 15px 20px;
            border-radius: 50%;
            cursor: pointer;
            font-size: 1.2em;
            border: 3px solid #6ab2e3;
            z-index: 999;
        }

        .navigation {
            display: flex;
            justify-content: space-between;
            margin-top: 20px;
            gap: 15px;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: #1c3a5c;
            color: white;
            border: none;
            padding: 15px 25px;
            border-radius: 15px;
            cursor: pointer;
            font-size: clamp(0.9em, 2.5vw, 1.1em);
            border: 3px solid #6ab2e3;
            font-weight: 700;
            flex: 1;
            min-width: 140px;
            position: relative;
            z-index: 10;
        }

        .nav-btn:hover {
            background: #6ab2e3;
            color: #1c3a5c;
        }

        .hidden {
            display: none;
        }

        .message {
            margin: 10px 0;
            padding: 12px 15px;
            border-radius: 10px;
            max-width: 80%;
            font-size: 0.95em;
            line-height: 1.4;
        }

        .user-message {
            background: #6ab2e3;
            color: black;
            margin-left: auto;
        }

        .support-message {
            background: #1c3a5c;
            color: white;
            margin-right: auto;
        }

        /* Адаптивность для персонажей */
        @media (max-width: 1200px) {
            .character-fullsize {
                width: 200px;
                height: 280px;
                bottom: 20px;
                right: 20px;
            }
            .chat-container, .chat-toggle {
                right: 250px;
            }
        }

        @media (max-width: 768px) {
            .character-fullsize {
                display: none; /* На мобильных скрываем больших персонажей */
            }
            .chat-container, .chat-toggle {
                right: 20px;
            }
            body { padding: 10px; }
            .grade-selection, .topic-selection, .content-area, .settings { padding: 20px; }
            .topics-grid { grid-template-columns: 1fr; }
            .character-selection { gap: 20px; }
            .character-img { width: 100px; height: 100px; }
            .navigation { flex-direction: column; }
            .nav-btn { width: 100%; }
        }

        /* Эффект при взаимодействии */
        .character-reaction {
            animation: happyJump 0.5s ease;
        }

        @keyframes happyJump {
            0%, 100% { transform: translateY(0px) scale(1); }
            50% { transform: translateY(-30px) scale(1.1); }
        }

        .character-sad {
            animation: sadShake 0.5s ease;
        }

        @keyframes sadShake {
            0%, 100% { transform: translateX(0px); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }

        /* Облачко речи для персонажа в теории */
        .character-speech {
            position: absolute;
            top: -100px;
            left: 50%;
            transform: translateX(-50%);
            background: white;
            padding: 15px 20px;
            border-radius: 20px;
            border: 3px solid #6ab2e3;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            font-weight: 700;
            color: #1c3a5c;
            max-width: 300px;
            text-align: center;
            animation: speechFloat 2s ease-in-out infinite;
            display: none;
        }

        .character-speech:after {
            content: '';
            position: absolute;
            bottom: -15px;
            left: 50%;
            transform: translateX(-50%);
            border-width: 15px 15px 0;
            border-style: solid;
            border-color: white transparent transparent;
        }

        @keyframes speechFloat {
            0%, 100% { transform: translateX(-50%) translateY(0px); }
            50% { transform: translateX(-50%) translateY(-5px); }
        }
    </style>
</head>
<body>
    <!-- Полноразмерный персонаж -->
    <div class="character-fullsize">
        <div class="character-model robot-model" id="fullsizeCharacter"></div>
        <div class="character-bubble" id="characterBubble"></div>
    </div>

    <div class="container">
        <div class="header">
            <h1>Smart Kid</h1>
            <p>Интерактивный тренажёр по математике для начальной школы</p>
        </div>

        <div class="grade-selection" id="gradeSelection">
            <h2>Выбери свой класс:</h2>
            <div class="grade-buttons">
                <button class="grade-btn" onclick="selectGrade(1)">1 класс</button>
                <button class="grade-btn" onclick="selectGrade(2)">2 класс</button>
                <button class="grade-btn" onclick="selectGrade(3)">3 класс</button>
                <button class="grade-btn" onclick="selectGrade(4)">4 класс</button>
            </div>
        </div>

        <div class="topic-selection hidden" id="topicSelection">
            <h2>Выбери тему:</h2>
            <div class="topics-grid" id="topicsGrid"></div>
            <div class="navigation">
                <button class="nav-btn" onclick="backToGrades()">← Назад к классам</button>
            </div>
        </div>

        <div class="content-area hidden" id="contentArea">
            <div class="score-display">Твои баллы: <span id="score">0</span></div>
            <div class="navigation">
                <button class="nav-btn" onclick="showTheory()">📚 Теория</button>
                <button class="nav-btn" onclick="showPractice()">✏️ Практика</button>
            </div>
            <div id="theoryContent" class="theory-content">
                <div class="theory-text" id="theoryText"></div>
            </div>
            <div id="practiceContent" class="practice-content hidden"></div>
            <div class="navigation">
                <button class="nav-btn" onclick="backToTopics()">← Назад к темам</button>
            </div>
        </div>

        <div class="settings" id="settings">
            <h2>Настройки</h2>
            <div class="character-selection">
                <div class="character-option selected" onclick="selectCharacter('robot')">
                    <img src="https://cdn-icons-png.flaticon.com/512/3163/3163478.png" alt="Робот" class="character-img">
                    <p>Робот</p>
                </div>
                <div class="character-option" onclick="selectCharacter('princess')">
                    <img src="https://cdn-icons-png.flaticon.com/512/4322/4322991.png" alt="Принцесса" class="character-img">
                    <p>Принцесса</p>
                </div>
            </div>
        </div>
    </div>

    <button class="chat-toggle" onclick="toggleChat()">💬</button>
    <div class="chat-container" id="chatContainer">
        <div class="chat-header">Чат с поддержкой</div>
        <div class="chat-messages" id="chatMessages">
            <div class="message support-message">Привет! Я тут, чтобы помочь тебе с математикой. Задавай любые вопросы! 😊</div>
        </div>
        <div class="chat-input">
            <input type="text" id="chatInput" placeholder="Напиши свой вопрос..." onkeypress="handleKeyPress(event)">
            <button onclick="sendMessage()">Отправить</button>
        </div>
    </div>

    <script>
        const curriculum = {
            1: [
                { title: "Числа от 1 до 5", theory: "Знакомимся с первыми числами! 1, 2, 3, 4, 5 - это наши первые друзья в мире математики. Каждое число показывает, сколько предметов мы видим.", tasks: [
                    { question: "Сколько будет 1 + 2?", answer: "3" }, { question: "Напиши число после 3", answer: "4" }, { question: "4 - 1 = ?", answer: "3" }, { question: "Какое число между 2 и 4?", answer: "3" }, { question: "2 + 2 = ?", answer: "4" }] },
                { title: "Числа от 1 до 10", theory: "Теперь изучаем числа до 10! Запоминаем, как они пишутся и как считаются. Счёт - это как лесенка: поднимаемся вверх!", tasks: [
                    { question: "3 + 4 = ?", answer: "7" }, { question: "Число после 7", answer: "8" }, { question: "6 - 3 = ?", answer: "3" }, { question: "Между 5 и 7", answer: "6" }, { question: "5 + 3 = ?", answer: "8" }] },
                { title: "Сложение до 10", theory: "Учимся складывать числа в пределах 10. Помни: сложение - это объединение! Как будто собираем друзей вместе.", tasks: [
                    { question: "2 + 3 = ?", answer: "5" }, { question: "4 + 5 = ?", answer: "9" }, { question: "1 + 7 = ?", answer: "8" }, { question: "6 + 2 = ?", answer: "8" }, { question: "3 + 6 = ?", answer: "9" }] },
                { title: "Вычитание до 10", theory: "Вычитание - это когда мы убираем часть предметов. Учимся вычитать в пределах 10. Представь, что раздаёшь конфеты друзьям!", tasks: [
                    { question: "7 - 2 = ?", answer: "5" }, { question: "9 - 4 = ?", answer: "5" }, { question: "8 - 3 = ?", answer: "5" }, { question: "6 - 1 = ?", answer: "5" }, { question: "10 - 5 = ?", answer: "5" }] },
                { title: "Сравнение чисел", theory: "Учимся сравнивать числа: больше, меньше или равно. Знаки: >, <. Представь пасть крокодила - он всегда хочет съесть большее число!", tasks: [
                    { question: "5 ? 3 (знак)", answer: ">" }, { question: "2 ? 4 (знак)", answer: "<" }, { question: "7 ? 7 (знак)", answer: "=" }, { question: "6 ? 8 (знак)", answer: "<" }, { question: "9 ? 5 (знак)", answer: ">" }] },
                { title: "Геометрические фигуры", theory: "Знакомимся с фигурами: круг, квадрат, треугольник, прямоугольник. Каждая фигура - как особый герой в стране математики!", tasks: [
                    { question: "Сколько углов у треугольника?", answer: "3" }, { question: "Фигура без углов", answer: "круг" }, { question: "Сторон у квадрата", answer: "4" }, { question: "Фигура с 4 равными сторонами", answer: "квадрат" }, { question: "2 длинные и 2 короткие стороны", answer: "прямоугольник" }] },
                { title: "Измерение длины", theory: "Учимся сравнивать длину: длиннее, короче, одинаковые. Используем линейку - наш волшебный измерительный инструмент!", tasks: [
                    { question: "Что длиннее: карандаш или ручка?", answer: "карандаш" }, { question: "Сантиметров в 1 дециметре", answer: "10" }, { question: "Инструмент для измерения", answer: "линейка" }, { question: "10 см = ? дм", answer: "1" }, { question: "5 см + 5 см = ?", answer: "10" }] },
                { title: "Время: часы", theory: "Знакомимся с часами. Учимся определять время по часам со стрелками. Маленькая стрелка - часы, большая - минуты!", tasks: [
                    { question: "Минут в 1 часе", answer: "60" }, { question: "Который час: стрелки на 12 и 3", answer: "3" }, { question: "Часов в сутках", answer: "24" }, { question: "Время 00:00", answer: "полночь" }, { question: "60 минут = ? час", answer: "1" }] },
                { title: "Монеты и рубли", theory: "Знакомимся с деньгами. Учимся считать монеты и рубли. Деньги помогают нам делать покупки и планировать расходы!", tasks: [
                    { question: "Копеек в 1 рубле", answer: "100" }, { question: "5 + 3 рубля = ?", answer: "8" }, { question: "Какая монета больше: 1 или 5 рублей?", answer: "5" }, { question: "10 - 4 рубля = ?", answer: "6" }, { question: "2 × 3 рубля = ?", answer: "6" }] },
                { title: "Решение простых задач", theory: "Учимся решать простые текстовые задачи на сложение и вычитание. Внимательно читаем условие и представляем ситуацию!", tasks: [
                    { question: "У Маши 3 яблока, у Миши 2. Сколько всего?", answer: "5" }, { question: "Было 7 конфет, 2 съели. Сколько осталось?", answer: "5" }, { question: "На ветке 5 птиц, прилетело 3. Сколько стало?", answer: "8" }, { question: "В корзине 6 грибов, 4 выбросили. Сколько осталось?", answer: "2" }, { question: "Купили 4 тетради, подарили 1. Сколько осталось?", answer: "3" }] },
                { title: "Чётные и нечётные", theory: "Знакомимся с чётными и нечётными числами. Чётные делятся на 2 без остатка. Их можно разбить на пары!", tasks: [
                    { question: "Чётное: 2 или 3?", answer: "2" }, { question: "Чётное между 5 и 7", answer: "6" }, { question: "3 - чётное или нечётное?", answer: "нечётное" }, { question: "8 - чётное или нечётное?", answer: "чётное" }, { question: "Самое маленькое чётное число", answer: "2" }] },
                { title: "Состав числа", theory: "Учим состав чисел. Например, 5 = 2 + 3, 5 = 4 + 1, 5 = 5 + 0. Числа дружат между собой и могут разбиваться на части!", tasks: [
                    { question: "4 = 2 + ?", answer: "2" }, { question: "6 = 3 + ?", answer: "3" }, { question: "7 = 5 + ?", answer: "2" }, { question: "8 = 4 + ?", answer: "4" }, { question: "9 = 6 + ?", answer: "3" }] },
                { title: "Порядок чисел", theory: "Учимся определять предыдущее и следующее число. Важно знать порядок! Как в очереди за мороженым - каждый знает, кто перед ним и кто после.", tasks: [
                    { question: "Число перед 8", answer: "7" }, { question: "Число после 4", answer: "5" }, { question: "Между 6 и 8", answer: "7" }, { question: "Числа от 3 до 7", answer: "3,4,5,6,7" }, { question: "Число перед 10", answer: "9" }] },
                { title: "Счёт десятками", theory: "Учимся считать десятками: 10, 20, 30... Это поможет с большими числами! Десятки - как пачки карандашей по 10 штук.", tasks: [
                    { question: "10 + 10 = ?", answer: "20" }, { question: "Сколько десятков в 30?", answer: "3" }, { question: "20 + 30 = ?", answer: "50" }, { question: "40 - 20 = ?", answer: "20" }, { question: "5 десятков = ?", answer: "50" }] },
                { title: "Простые закономерности", theory: "Учимся находить закономерности в последовательностях чисел и фигур. Это как разгадывать секретный код математики!", tasks: [
                    { question: "Продолжи: 2, 4, 6, ?", answer: "8" }, { question: "Продолжи: 5, 10, 15, ?", answer: "20" }, { question: "Пропущено: 3, 6, ?, 12", answer: "9" }, { question: "Продолжи: 10, 8, 6, ?", answer: "4" }, { question: "Лишнее: 2, 4, 5, 6, 8", answer: "5" }] }
            ]
        };

        let currentGrade = null;
        let currentTopic = null;
        let currentCharacter = 'robot';
        let score = 0;
        let currentTasks = [];
        let characterMessages = [
            "Привет! Я помогу тебе разобраться с математикой!",
            "Математика - это весело! Давай учиться вместе!",
            "Отличная работа! Ты настоящий математик!",
            "Не переживай, если ошибешься. Мы все учимся!",
            "Смотри, как я радуюсь твоим успехам!",
            "Ещё немного практики - и будешь считать быстрее меня!",
            "Математика нужна везде: в магазине, в играх, в жизни!",
            "Ты делаешь большие успехи! Продолжай в том же духе!"
        ];

        function selectGrade(grade) {
            currentGrade = grade;
            document.getElementById('gradeSelection').classList.add('hidden');
            document.getElementById('topicSelection').classList.remove('hidden');
            showCharacterBubble("Выбери тему для изучения! Я готов помочь!");
            
            const topicsGrid = document.getElementById('topicsGrid');
            topicsGrid.innerHTML = '';
            
            curriculum[grade].forEach((topic, index) => {
                const topicCard = document.createElement('div');
                topicCard.className = 'topic-card';
                topicCard.innerHTML = `
                    <h3>${topic.title}</h3>
                    <p>${topic.theory.substring(0, 60)}...</p>
                `;
                topicCard.onclick = () => selectTopic(index);
                topicsGrid.appendChild(topicCard);
            });
        }

        function backToGrades() {
            document.getElementById('topicSelection').classList.add('hidden');
            document.getElementById('contentArea').classList.add('hidden');
            document.getElementById('gradeSelection').classList.remove('hidden');
            showCharacterBubble("Выбирай класс и начинаем учиться!");
        }

        function selectTopic(topicIndex) {
            currentTopic = topicIndex;
            document.getElementById('topicSelection').classList.add('hidden');
            document.getElementById('contentArea').classList.remove('hidden');
            
            const topic = curriculum[currentGrade][currentTopic];
            showCharacterBubble(`Отлично! Тема "${topic.title}" очень интересная!`);
            
            showTheory();
            updateScoreDisplay();
        }

        function backToTopics() {
            document.getElementById('contentArea').classList.add('hidden');
            document.getElementById('topicSelection').classList.remove('hidden');
            showCharacterBubble("Выбирай следующую тему для изучения!");
        }

        function showTheory() {
            document.getElementById('practiceContent').classList.add('hidden');
            document.getElementById('theoryContent').classList.remove('hidden');
            
            const topic = curriculum[currentGrade][currentTopic];
            
            document.getElementById('theoryText').innerHTML = `
                <h3 style="color: #1c3a5c; margin-bottom: 15px;">${topic.title}</h3>
                <div style="font-size: 1.2em; line-height: 1.8;">
                    ${topic.theory}
                </div>
            `;
            
            showCharacterBubble("Внимательно изучи теорию, а потом перейдём к практике!");
        }

        function showPractice() {
            document.getElementById('theoryContent').classList.add('hidden');
            document.getElementById('practiceContent').classList.remove('hidden');
            showCharacterBubble("Пора проверить знания! Решай задания внимательно!");
            
            const topic = curriculum[currentGrade][currentTopic];
            currentTasks = [...topic.tasks];
            
            const practiceHTML = topic.tasks.map((task, index) => `
                <div class="task" id="task${index}">
                    <h4>Задание ${index + 1}</h4>
                    <p>${task.question}</p>
                    <input type="text" id="answer${index}" placeholder="Твой ответ">
                    <button onclick="checkAnswer(${index})">Проверить</button>
                    <div id="result${index}" class="result"></div>
                </div>
            `).join('');
            
            document.getElementById('practiceContent').innerHTML = practiceHTML;
        }

        function checkAnswer(taskIndex) {
            const userAnswer = document.getElementById(`answer${taskIndex}`).value.trim().toLowerCase();
            const correctAnswer = currentTasks[taskIndex].answer.toLowerCase();
            const resultDiv = document.getElementById(`result${taskIndex}`);
            const character = document.getElementById('fullsizeCharacter');
            
            if (userAnswer === correctAnswer || (correctAnswer === '-' && userAnswer !== '')) {
                resultDiv.textContent = '✅ Правильно! Молодец!';
                resultDiv.className = 'result correct';
                score += 10;
                updateScoreDisplay();
                
                // Анимация радости персонажа
                character.classList.add('character-reaction');
                setTimeout(() => {
                    character.classList.remove('character-reaction');
                }, 500);
                
                showCharacterBubble("Ура! Правильно! Ты настоящий математик! 🎉");
                
                // Случайные радостные сообщения
                const happyMessages = [
                    "Отлично! Ты справился!",
                    "Браво! Ты всё понимаешь!",
                    "Так держать! Продолжай в том же духе!",
                    "Ты меня впечатляешь!",
                    "Математика тебе покоряется!"
                ];
                setTimeout(() => {
                    showCharacterBubble(happyMessages[Math.floor(Math.random() * happyMessages.length)]);
                }, 2000);
            } else {
                resultDiv.textContent = '❌ Попробуй ещё раз! Ты сможешь!';
                resultDiv.className = 'result incorrect';
                
                // Анимация грусти персонажа
                character.classList.add('character-sad');
                setTimeout(() => {
                    character.classList.remove('character-sad');
                }, 500);
                
                showCharacterBubble("Не расстраивайся! Попробуй ещё раз, я верю в тебя! 💪");
                
                // Случайные поддерживающие сообщения
                const supportMessages = [
                    "Попробуй подумать ещё раз!",
                    "Ошибки помогают нам учиться!",
                    "Ты почти у цели!",
                    "Давай разберём вместе?",
                    "Не сдавайся! У тебя получится!"
                ];
                setTimeout(() => {
                    showCharacterBubble(supportMessages[Math.floor(Math.random() * supportMessages.length)]);
                }, 2000);
            }
        }

        function updateScoreDisplay() {
            document.getElementById('score').textContent = score;
            
            // Реакция персонажа на набор баллов
            const character = document.getElementById('fullsizeCharacter');
            if (score % 50 === 0 && score > 0) {
                character.classList.add('character-reaction');
                setTimeout(() => {
                    character.classList.remove('character-reaction');
                }, 500);
                showCharacterBubble(`Ура! У тебя уже ${score} баллов! Ты супер! 🏆`);
            }
        }

        function selectCharacter(character) {
            currentCharacter = character;
            document.querySelectorAll('.character-option').forEach(opt => {
                opt.classList.remove('selected');
            });
            event.currentTarget.classList.add('selected');
            
            // Меняем полноразмерного персонажа
            const fullsizeChar = document.getElementById('fullsizeCharacter');
            fullsizeChar.className = 'character-model ' + character + '-model';
            
            // Анимация перехода
            fullsizeChar.style.transform = 'scale(0.8)';
            setTimeout(() => {
                fullsizeChar.style.transform = 'scale(1)';
            }, 300);
            
            const characterName = character === 'robot' ? 'Робот' : 'Принцесса';
            showCharacterBubble(`Привет! Я ${characterName} и я помогу тебе с математикой!`);
        }

        function showCharacterBubble(message) {
            const bubble = document.getElementById('characterBubble');
            bubble.textContent = message;
            bubble.style.display = 'block';
            
            // Скрываем облачко через 5 секунд
            setTimeout(() => {
                bubble.style.display = 'none';
            }, 5000);
        }

        function toggleChat() {
            const chat = document.getElementById('chatContainer');
            chat.style.display = chat.style.display === 'flex' ? 'none' : 'flex';
        }

        function sendMessage() {
            const input = document.getElementById('chatInput');
            const message = input.value.trim();
            const messagesContainer = document.getElementById('chatMessages');
            
            if (message) {
                const userMessage = document.createElement('div');
                userMessage.className = 'message user-message';
                userMessage.textContent = message;
                messagesContainer.appendChild(userMessage);
                
                input.value = '';
                
                // Реакция персонажа на сообщение
                const character = document.getElementById('fullsizeCharacter');
                character.classList.add('character-reaction');
                setTimeout(() => {
                    character.classList.remove('character-reaction');
                }, 300);
                
                showCharacterBubble("Спасибо за вопрос! Давай разберём его вместе!");
                
                setTimeout(() => {
                    const supportMessage = document.createElement('div');
                    supportMessage.className = 'message support-message';
                    supportMessage.textContent = getSupportResponse(message);
                    messagesContainer.appendChild(supportMessage);
                    messagesContainer.scrollTop = messagesContainer.scrollHeight;
                }, 1000);
                
                messagesContainer.scrollTop = messagesContainer.scrollHeight;
            }
        }

        function handleKeyPress(event) {
            if (event.key === 'Enter') {
                sendMessage();
            }
        }

        function getSupportResponse(message) {
            const responses = [
                "Отличный вопрос! Давай разберём его вместе.",
                "Математика - это интересно! Попробуй решить задачу шаг за шагом.",
                "Не переживай, если не получается с первого раза. Практика - ключ к успеху!",
                "Помни: даже великие математики ошибались. Главное - не сдаваться!",
                "Отличная попытка! Хочешь, разберём эту тему подробнее?"
            ];
            return responses[Math.floor(Math.random() * responses.length)];
        }

        // Инициализация персонажа при загрузке
        document.addEventListener('DOMContentLoaded', function() {
            updateScoreDisplay();
            
            // Случайное сообщение от персонажа при загрузке
            setTimeout(() => {
                showCharacterBubble(characterMessages[Math.floor(Math.random() * characterMessages.length)]);
            }, 1000);
            
            // Периодические сообщения от персонажа
            setInterval(() => {
                if (Math.random() > 0.7) { // 30% шанс
                    const randomMessage = characterMessages[Math.floor(Math.random() * characterMessages.length)];
                    showCharacterBubble(randomMessage);
                }
            }, 15000);
        });

        // Анимация персонажа при движении мыши
        document.addEventListener('mousemove', function(e) {
            const character = document.getElementById('fullsizeCharacter');
            const x = e.clientX / window.innerWidth;
            const y = e.clientY / window.innerHeight;
            
            // Слежение персонажа за курсором
            character.style.transform = `translate(${(x - 0.5) * 10}px, ${(y - 0.5) * 10}px)`;
        });
    </script>
</body>
</html># smartkid
