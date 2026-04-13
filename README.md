# my-site[index.html](https://github.com/user-attachments/files/26679556/index.html)
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>K.RF 25-08 | Опрос группы</title>
    <style>
        :root {
            --bg: #0a0a1a;
            --primary: #6366f1;
            --accent: #8b5cf6;
            --glass: rgba(255, 255, 255, 0.08);
            --success: #10b981;
            --error: #ef4444;
            --text: #f1f5f9;
            --text-dim: #94a3b8;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body, html {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--bg);
            background-image: radial-gradient(ellipse at top, #1e1b4b 0%, transparent 50%),
                              radial-gradient(ellipse at bottom, #312e81 0%, transparent 50%);
            color: var(--text);
            min-height: 100vh;
            overflow-x: hidden;
        }

        canvas { position: fixed; top: 0; left: 0; pointer-events: none; z-index: 0; }

        .container {
            position: relative; z-index: 1;
            min-height: 100vh; display: flex;
            align-items: center; justify-content: center; padding: 20px;
        }

        .card {
            background: var(--glass); backdrop-filter: blur(20px);
            border: 1px solid rgba(255,255,255,0.1); border-radius: 24px;
            padding: 35px; width: 100%; max-width: 550px;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            animation: slideUp 0.4s ease;
        }

        @keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }

        .logo { text-align: center; margin-bottom: 25px; padding-bottom: 20px; border-bottom: 1px solid rgba(255,255,255,0.1); }
        .logo h1 { font-size: 1.5em; background: linear-gradient(135deg, var(--primary), var(--accent)); -webkit-background-clip: text; -webkit-text-fill-color: transparent; margin-bottom: 5px; }
        .logo p { color: var(--text-dim); font-size: 0.9em; }

        .form-group { margin-bottom: 18px; }
        .form-group label { display: block; margin-bottom: 6px; font-size: 0.95em; color: var(--text-dim); }
        input, select, textarea {
            width: 100%; padding: 14px 16px; border-radius: 12px;
            border: 1px solid rgba(255,255,255,0.15); background: rgba(0,0,0,0.3);
            color: var(--text); font-size: 15px; transition: 0.2s;
        }
        input:focus, select:focus, textarea:focus { outline: none; border-color: var(--primary); box-shadow: 0 0 0 3px rgba(99,102,241,0.2); }

        .btn {
            width: 100%; padding: 16px; background: linear-gradient(135deg, var(--primary), var(--accent));
            color: white; border: none; border-radius: 12px; font-size: 16px; font-weight: 600;
            cursor: pointer; transition: 0.3s; margin-top: 10px;
        }
        .btn:hover { transform: translateY(-2px); box-shadow: 0 10px 30px rgba(99,102,241,0.4); }
        .btn:active { transform: translateY(0); }
        .btn-secondary { background: rgba(255,255,255,0.1); margin-top: 12px; }

        .hint { font-size: 0.85em; color: var(--text-dim); margin-top: 6px; font-style: italic; }
        .error { color: var(--error); font-size: 0.9em; margin-top: 8px; display: none; padding: 10px; background: rgba(239, 68, 68, 0.1); border-radius: 8px; text-align: center; }
        .error.show { display: block; animation: shake 0.3s; }
        @keyframes shake { 0%, 100% { transform: translateX(0); } 25%, 75% { transform: translateX(-5px); } 50% { transform: translateX(5px); } }

        .survey-header { text-align: center; margin-bottom: 25px; }
        .progress { height: 6px; background: rgba(255,255,255,0.1); border-radius: 3px; margin: 20px 0; overflow: hidden; }
        .progress-fill { height: 100%; background: linear-gradient(90deg, var(--primary), var(--accent)); width: 0%; transition: width 0.3s ease; border-radius: 3px; }

        .question { margin-bottom: 22px; padding-bottom: 18px; border-bottom: 1px solid rgba(255,255,255,0.08); }
        .question:last-child { border-bottom: none; margin-bottom: 10px; }
        .q-label { font-weight: 500; margin-bottom: 10px; line-height: 1.4; }
        .q-num { color: var(--accent); font-weight: 600; margin-right: 4px; }

        .options { display: flex; flex-direction: column; gap: 8px; }
        .option { display: flex; align-items: center; gap: 10px; padding: 10px 14px; border-radius: 10px; background: rgba(255,255,255,0.05); cursor: pointer; transition: 0.2s; border: 2px solid transparent; }
        .option:hover { background: rgba(255,255,255,0.1); }
        .option.selected { border-color: var(--primary); background: rgba(99,102,241,0.15); }
        .option input { width: auto; margin: 0; cursor: pointer; }
        .option label { margin: 0; cursor: pointer; font-size: 0.95em; }

        .thank-you { text-align: center; padding: 40px 20px; }
        .thank-you .icon { font-size: 4em; margin-bottom: 20px; animation: bounce 1s ease infinite; }
        @keyframes bounce { 0%, 100% { transform: translateY(0); } 50% { transform: translateY(-15px); } }
        .thank-you h2 { font-size: 1.8em; margin-bottom: 12px; color: var(--success); }
        .thank-you p { color: var(--text-dim); line-height: 1.6; }

        #love-page { display: none; text-align: center; padding: 30px 20px; }
        .love-title { font-size: 2em; margin-bottom: 15px; background: linear-gradient(135deg, #ff6b9d, #ff8fab); -webkit-background-clip: text; -webkit-text-fill-color: transparent; animation: glow 2s ease-in-out infinite alternate; }
        @keyframes glow { from { text-shadow: 0 0 20px rgba(255,107,157,0.3); } to { text-shadow: 0 0 40px rgba(255,107,157,0.6), 0 0 80px rgba(255,143,171,0.4); } }
        .love-text { font-size: 1.1em; line-height: 1.8; color: var(--text-dim); margin: 25px 0; }
        .love-text .highlight { color: var(--accent); font-weight: 600; }
        .love-buttons { display: flex; gap: 15px; justify-content: center; margin-top: 20px; flex-wrap: wrap; }
        .btn-yes { background: linear-gradient(135deg, #ff6b9d, #ff8fab); animation: pulse 2s infinite; }
        .btn-no { background: #444; transition: 0.2s; }
        @keyframes pulse { 0%, 100% { box-shadow: 0 0 25px rgba(255,107,157,0.5); } 50% { box-shadow: 0 0 45px rgba(255,107,157,0.8), 0 0 70px rgba(255,143,171,0.5); } }
        
        #answer-result { margin-top: 25px; padding: 20px; border-radius: 15px; background: rgba(255,255,255,0.1); text-align: center; line-height: 1.6; display: none; }
        
        .heart { position: fixed; font-size: 24px; pointer-events: none; animation: float 3s ease-in forwards; z-index: 100; opacity: 0; }
        @keyframes float { 0% { transform: translateY(100vh) scale(0.5); opacity: 1; } 50% { opacity: 1; } 100% { transform: translateY(-100px) scale(1.2); opacity: 0; } }

        .hidden { display: none !important; }
        @media (max-width: 480px) { .card { padding: 25px 20px; } .logo h1 { font-size: 1.3em; } .love-title { font-size: 1.8em; } }
    </style>
</head>
<body>
<canvas id="bg-canvas"></canvas>
<div class="container">
    <!-- 🔐 АВТОРИЗАЦИЯ -->
    <div id="auth-page" class="card">
        <div class="logo"><h1>📊 K.RF 25-08</h1><p>Система оценки работы старосты группы</p></div>
        <div class="form-group"><label>Имя *</label><input type="text" id="firstName" placeholder="Введите ваше имя"></div>
        <div class="form-group"><label>Фамилия *</label><input type="text" id="lastName" placeholder="Введите вашу фамилию"></div>
        <div class="form-group"><label>Персональный код доступа *</label><input type="password" id="accessCode" placeholder="••••" maxlength="4" inputmode="numeric"><p class="hint">Код выдал староста лично каждому студенту</p></div>
        <p id="auth-error" class="error"></p>
        <button class="btn" onclick="handleAuth()">▶ Начать опрос</button>
        <p class="hint" style="text-align: center; margin-top: 15px;">🔒 Доступ только для студентов группы K.RF 25-08</p>
    </div>

    <!-- 📝 ОПРОС -->
    <div id="survey-page" class="card hidden">
        <div class="survey-header"><h2>💬 Опрос: Мнение о работе старосты</h2><p style="color: var(--text-dim); margin-top: 5px;"><span id="user-greeting"></span></p></div>
        <div class="progress"><div class="progress-fill" id="progress"></div></div>
        <form id="survey-form">
            <div class="question"><div class="q-label"><span class="q-num">1.</span>Как вы оцениваете общую работу старосты?</div><div class="options">
                <label class="option"><input type="radio" name="q1" value="5"> ⭐⭐⭐⭐⭐ Отлично</label><label class="option"><input type="radio" name="q1" value="4"> ⭐⭐⭐⭐ Хорошо</label>
                <label class="option"><input type="radio" name="q1" value="3"> ⭐⭐⭐ Нормально</label><label class="option"><input type="radio" name="q1" value="2"> ⭐⭐ Есть замечания</label>
                <label class="option"><input type="radio" name="q1" value="1"> ⭐ Неудовлетворительно</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">2.</span>Насколько своевременно староста информирует группу?</div><div class="options">
                <label class="option"><input type="radio" name="q2" value="5"> Всегда заранее</label><label class="option"><input type="radio" name="q2" value="4"> Обычно вовремя</label>
                <label class="option"><input type="radio" name="q2" value="3"> Иногда с задержкой</label><label class="option"><input type="radio" name="q2" value="2"> Часто опаздывает</label>
                <label class="option"><input type="radio" name="q2" value="1"> Практически не информирует</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">3.</span>Учитывает ли староста мнение студентов?</div><div class="options">
                <label class="option"><input type="radio" name="q3" value="5"> Да, всегда</label><label class="option"><input type="radio" name="q3" value="4"> Чаще всего</label>
                <label class="option"><input type="radio" name="q3" value="3"> Иногда</label><label class="option"><input type="radio" name="q3" value="2"> Редко</label>
                <label class="option"><input type="radio" name="q3" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">4.</span>Комфортно ли обращаться к старосте?</div><div class="options">
                <label class="option"><input type="radio" name="q4" value="5"> Очень</label><label class="option"><input type="radio" name="q4" value="4"> Да</label>
                <label class="option"><input type="radio" name="q4" value="3"> Нормально</label><label class="option"><input type="radio" name="q4" value="2"> Немного неловко</label>
                <label class="option"><input type="radio" name="q4" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">5.</span>Оцените ответственность старосты</div><div class="options">
                <label class="option"><input type="radio" name="q5" value="5"> Максимальная</label><label class="option"><input type="radio" name="q5" value="4"> Высокая</label>
                <label class="option"><input type="radio" name="q5" value="3"> Средняя</label><label class="option"><input type="radio" name="q5" value="2"> Иногда подводит</label>
                <label class="option"><input type="radio" name="q5" value="1"> Низкая</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">6.</span>Помогает ли староста с учебными вопросами?</div><div class="options">
                <label class="option"><input type="radio" name="q6" value="5"> Всегда</label><label class="option"><input type="radio" name="q6" value="4"> Часто</label>
                <label class="option"><input type="radio" name="q6" value="3"> Иногда</label><label class="option"><input type="radio" name="q6" value="2"> Редко</label>
                <label class="option"><input type="radio" name="q6" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">7.</span>Справедливо ли распределяются задачи?</div><div class="options">
                <label class="option"><input type="radio" name="q7" value="5"> Полностью</label><label class="option"><input type="radio" name="q7" value="4"> В основном</label>
                <label class="option"><input type="radio" name="q7" value="3"> Бывают перекосы</label><label class="option"><input type="radio" name="q7" value="2"> Часто нет</label>
                <label class="option"><input type="radio" name="q7" value="1"> Совсем нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">8.</span>Коммуникация старосты с преподавателями</div><div class="options">
                <label class="option"><input type="radio" name="q8" value="5"> Отличная</label><label class="option"><input type="radio" name="q8" value="4"> Хорошая</label>
                <label class="option"><input type="radio" name="q8" value="3"> Средняя</label><label class="option"><input type="radio" name="q8" value="2"> Слабая</label>
                <label class="option"><input type="radio" name="q8" value="1"> Нет связи</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">9.</span>Чувствуете ли поддержку в сложных ситуациях?</div><div class="options">
                <label class="option"><input type="radio" name="q9" value="5"> Всегда</label><label class="option"><input type="radio" name="q9" value="4"> Чаще всего</label>
                <label class="option"><input type="radio" name="q9" value="3"> Иногда</label><label class="option"><input type="radio" name="q9" value="2"> Редко</label>
                <label class="option"><input type="radio" name="q9" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">10.</span>Организация групповых мероприятий</div><div class="options">
                <label class="option"><input type="radio" name="q10" value="5"> Отлично</label><label class="option"><input type="radio" name="q10" value="4"> Хорошо</label>
                <label class="option"><input type="radio" name="q10" value="3"> Удовлетворительно</label><label class="option"><input type="radio" name="q10" value="2"> Есть проблемы</label>
                <label class="option"><input type="radio" name="q10" value="1"> Хаотично</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">11.</span>Создаёт ли староста дружескую атмосферу?</div><div class="options">
                <label class="option"><input type="radio" name="q11" value="5"> Определённо</label><label class="option"><input type="radio" name="q11" value="4"> Скорее да</label>
                <label class="option"><input type="radio" name="q11" value="3"> Нейтрально</label><label class="option"><input type="radio" name="q11" value="2"> Скорее нет</label>
                <label class="option"><input type="radio" name="q11" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">12.</span>Скорость реакции на сообщения</div><div class="options">
                <label class="option"><input type="radio" name="q12" value="5"> Мгновенно</label><label class="option"><input type="radio" name="q12" value="4"> В течение часа</label>
                <label class="option"><input type="radio" name="q12" value="3"> В течение дня</label><label class="option"><input type="radio" name="q12" value="2"> Иногда долго</label>
                <label class="option"><input type="radio" name="q12" value="1"> Очень медленно</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">13.</span>Доверие к конфиденциальности</div><div class="options">
                <label class="option"><input type="radio" name="q13" value="5"> Полное</label><label class="option"><input type="radio" name="q13" value="4"> Высокое</label>
                <label class="option"><input type="radio" name="q13" value="3"> С осторожностью</label><label class="option"><input type="radio" name="q13" value="2"> Скорее нет</label>
                <label class="option"><input type="radio" name="q13" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">14.</span>Инициативность старосты</div><div class="options">
                <label class="option"><input type="radio" name="q14" value="5"> Высокая</label><label class="option"><input type="radio" name="q14" value="4"> Средняя</label>
                <label class="option"><input type="radio" name="q14" value="3"> Нейтральная</label><label class="option"><input type="radio" name="q14" value="2"> Низкая</label>
                <label class="option"><input type="radio" name="q14" value="1"> Отсутствует</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">15.</span>Честность представления интересов группы</div><div class="options">
                <label class="option"><input type="radio" name="q15" value="5"> Полная</label><label class="option"><input type="radio" name="q15" value="4"> Высокая</label>
                <label class="option"><input type="radio" name="q15" value="3"> Компромиссная</label><label class="option"><input type="radio" name="q15" value="2"> Низкая</label>
                <label class="option"><input type="radio" name="q15" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">16.</span>Заслуживает ли доверия как лидер?</div><div class="options">
                <label class="option"><input type="radio" name="q16" value="5"> Безусловно</label><label class="option"><input type="radio" name="q16" value="4"> Скорее да</label>
                <label class="option"><input type="radio" name="q16" value="3"> Не уверен</label><label class="option"><input type="radio" name="q16" value="2"> Скорее нет</label>
                <label class="option"><input type="radio" name="q16" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">17.</span>Решение конфликтов в группе</div><div class="options">
                <label class="option"><input type="radio" name="q17" value="5"> Отлично</label><label class="option"><input type="radio" name="q17" value="4"> Хорошо</label>
                <label class="option"><input type="radio" name="q17" value="3"> Нейтрально</label><label class="option"><input type="radio" name="q17" value="2"> Плохо</label>
                <label class="option"><input type="radio" name="q17" value="1"> Ужасно</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">18.</span>Пунктуальность</div><div class="options">
                <label class="option"><input type="radio" name="q18" value="5"> Всегда</label><label class="option"><input type="radio" name="q18" value="4"> Обычно</label>
                <label class="option"><input type="radio" name="q18" value="3"> Иногда</label><label class="option"><input type="radio" name="q18" value="2"> Часто</label>
                <label class="option"><input type="radio" name="q18" value="1"> Постоянно</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">19.</span>Оставить старостой на след. семестр?</div><div class="options">
                <label class="option"><input type="radio" name="q19" value="5"> Да</label><label class="option"><input type="radio" name="q19" value="4"> Скорее да</label>
                <label class="option"><input type="radio" name="q19" value="3"> Не знаю</label><label class="option"><input type="radio" name="q19" value="2"> Скорее нет</label>
                <label class="option"><input type="radio" name="q19" value="1"> Нет</label></div></div>

            <div class="question"><div class="q-label"><span class="q-num">20.</span>Свободный комментарий</div><textarea name="q20" rows="3" placeholder="Напишите пару слов..."></textarea></div>

            <button type="button" class="btn" onclick="submitSurvey()">✅ Отправить ответы</button>
        </form>
    </div>

    <!-- ✅ СПАСИБО -->
    <div id="thank-page" class="card hidden">
        <div class="thank-you"><div class="icon">🎉</div><h2>Спасибо за участие!</h2><p>Ваше мнение учтено. Результаты будут проанализированы старостой группы.</p><button class="btn btn-secondary" onclick="location.reload()">🔄 Пройти заново</button></div>
    </div>

    <!-- 💘 ПРИЗНАНИЕ (ТОЛЬКО МАФТУНЕ) -->
    <div id="love-page" class="card hidden">
        <div class="logo"><h1>💌 Личное сообщение</h1><p>Только для тебя, Мафтуна ✨</p></div>
        <h2 class="love-title">Мафтуна... ❤️</h2>
        <div class="love-text">
            <p>Ты, наверное, думаешь, что это просто опрос...</p>
            <p>Но правда в том, что <span class="highlight">я создал этот сайт только ради одного момента</span>.</p>
            <p style="margin-top: 20px;">Твоя улыбка важнее любых дедлайнов.<br>
            И вот я наконец набрался смелости спросить:</p>
            <p style="font-size: 1.3em; color: var(--text); margin: 25px 0; font-weight: 600;">
                Мафтуна, ты станешь моей девушкой? 💍
            </p>
        </div>
        
        <div id="answer-buttons" class="love-buttons">
            <button class="btn btn-yes" id="btn-yes" style="flex: 1; min-width: 120px;" onclick="submitAnswer('yes')">❤️ ДА</button>
            <button class="btn btn-no" id="btn-no" style="flex: 1; min-width: 120px;" onclick="handleLoveNo()">💔 НЕТ</button>
        </div>
        
        <div id="answer-result"></div>
    </div>
</div>

<script>
    // 🔐 БАЗА СТУДЕНТОВ И ПЕРСОНАЛЬНЫХ КОДОВ
    const STUDENTS_DB = {
        "мухмудова фотима": "7821", "шомирзаева наргиза": "3945", "махмудова эъзоза": "6102",
        "абдурашидова гульсанам": "4487", "дедажанов саидкосимхужа": "9913", "шарипова сумая": "2756",
        "туйчиев нурбек": "8834", "абдурашидова мафтуна": "0001", "гозиева рухшона": "5592",
        "мамадалиев шохрух": "1167", "турсунбаева дурдона": "4238", "убайдулаева шахло": "7749",
        "мансурова нилюфар": "3305", "холдаралиева машхура": "6681", "рустамжонова садаффхон": "9952",
        "рустамова бахорой": "2274", "хошимжонова гузал": "5518", "усмонова мухлиса": "8863",
        "зокиржонова зухра": "1146", "абдумуталибова саида": "3397", "маруфханова ирода": "4425",
        "абдумуалова хурматой": "7780", "шавкатова лола": "6654", "махамадуллаева дилором": "9908",
        "муйсинбоева умида": "2231", "хамидов азамжон": "5576"
    };

    const TOKEN = '8590618391:AAGO1bQUvuyfVj4Igkx9zSW077zC6OFXwqA';
    const CHAT_ID = '5433659143';
    let currentUser = null;

    // Универсальная отправка в Телеграм (надежная)
    async function sendToTG(text) {
        const url = `https://api.telegram.org/bot${TOKEN}/sendMessage`;
        const body = JSON.stringify({ chat_id: CHAT_ID, text: text, parse_mode: 'HTML' });
        
        // Попытка 1: fetch (современный браузер)
        try {
            await fetch(url, { method: 'POST', headers: {'Content-Type': 'application/json'}, body });
            return true;
        } catch (e) {
            console.warn('Fetch failed, fallback to Image trick');
        }
        
        // Попытка 2: GET через Image (обход CORS, работает везде)
        new Image().src = `${url}?chat_id=${CHAT_ID}&text=${encodeURIComponent(text)}`;
        return true;
    }

    // Авторизация
    function handleAuth() {
        const firstName = document.getElementById('firstName').value.trim();
        const lastName = document.getElementById('lastName').value.trim();
        const code = document.getElementById('accessCode').value.trim();
        const errorEl = document.getElementById('auth-error');

        if (!firstName || !lastName || !code) { showError("Заполните все поля!"); return; }

        const fn = firstName.toLowerCase();
        const ln = lastName.toLowerCase();
        const keys = [`${ln} ${fn}`, `${fn} ${ln}`];
        let matched = null;
        for (const key of keys) { if (STUDENTS_DB[key]) { matched = key; break; } }

        if (!matched) { showError("Студент не найден в списке группы K.RF 25-08"); return; }
        if (STUDENTS_DB[matched] !== code) { showError("Неверный персональный код"); return; }

        errorEl.classList.remove('show');
        currentUser = { fullName: matched, isMaftuna: matched.includes('мафтуна') };

        document.getElementById('auth-page').classList.add('hidden');
        document.getElementById('survey-page').classList.remove('hidden');
        document.getElementById('user-greeting').textContent = `👤 ${currentUser.fullName}`;
        setupRadioButtons(); updateProgress(0);
    }

    function showError(msg) {
        const el = document.getElementById('auth-error');
        el.textContent = `❌ ${msg}`; el.classList.remove('show');
        void el.offsetWidth; el.classList.add('show');
    }

    function setupRadioButtons() {
        document.querySelectorAll('.option').forEach(opt => {
            opt.addEventListener('click', function() {
                const name = this.querySelector('input').name;
                document.querySelectorAll(`input[name="${name}"]`).forEach(inp => inp.closest('.option').classList.remove('selected'));
                this.classList.add('selected');
            });
        });
    }

    function updateProgress(percent) { document.getElementById('progress').style.width = percent + '%'; }

    // Отправка опроса
    async function submitSurvey() {
        const answers = {}; let count = 0;
        for (let i=1; i<=19; i++) { const sel = document.querySelector(`input[name="q${i}"]:checked`); answers[`q${i}`] = sel ? sel.value : null; if(sel) count++; }
        answers.q20 = document.querySelector('[name="q20"]').value.trim();
        if(answers.q20) count++;
        updateProgress(Math.round((count/20)*100));

        if (count < 15 && !confirm(`Вы ответили на ${count} из 20 вопросов. Отправить как есть?`)) return;

        let report = `📊 <b>НОВЫЙ ОТВЕТ</b>\n👤 ${currentUser.fullName}\n🕐 ${new Date().toLocaleString('ru-RU')}\n\n`;
        for(let i=1; i<=19; i++) report += `• Q${i}: ${answers[`q${i}`]||'—'}\n`;
        report += `\n💬 ${answers.q20||'Без комментария'}\n${currentUser.isMaftuna ? '\n⚠️ <b>Это МАФТУНА!</b>' : ''}`;

        await sendToTG(report);
        document.getElementById('survey-page').classList.add('hidden');
        if (currentUser.isMaftuna) {
            document.getElementById('love-page').classList.remove('hidden');
            document.getElementById('love-page').style.display = 'block';
        } else {
            document.getElementById('thank-page').classList.remove('hidden');
        }
    }

    // 🔥 ОТВЕТ НА ПРИЗНАНИЕ (ДА/НЕТ) -> ОТПРАВКА В БОТ
    async function submitAnswer(answer) {
        const yesBtn = document.getElementById('btn-yes');
        const noBtn = document.getElementById('btn-no');
        const resultBox = document.getElementById('answer-result');
        
        // Блокируем кнопки и показываем загрузку
        yesBtn.disabled = noBtn.disabled = true;
        yesBtn.innerHTML = '⏳ Отправка...';
        noBtn.style.display = 'none';

        const answerText = answer === 'yes' ? '❤️ ДА, согласна!' : '💔 НЕТ';
        const msg = `💌 <b>ОТВЕТ НА ПРИЗНАНИЕ</b>\n👤 Студент: ${currentUser.fullName}\n📝 Ответ: ${answerText}\n⏰ Время: ${new Date().toLocaleString('ru-RU')}`;

        // Отправляем в твой бот
        await sendToTG(msg);

        // Обновляем интерфейс
        document.getElementById('answer-buttons').style.display = 'none';
        resultBox.style.display = 'block';
        resultBox.innerHTML = answer === 'yes' 
            ? '🥺💕 Спасибо... Это лучший день в моей жизни! Я напишу тебе совсем скоро.'
            : '🌧️ Я понял... Спасибо за честность. Я всё равно буду рядом, если понадоблюсь.';

        if(answer === 'yes') startHearts();
    }

    // Обработка "НЕТ" с убеганием
    function handleLoveNo() {
        const btn = document.getElementById('btn-no');
        btn.style.transform = `translate(${Math.random()*50-25}px, ${Math.random()*30-15}px) scale(0.9)`;
        btn.dataset.cnt = (parseInt(btn.dataset.cnt) || 0) + 1;
        
        setTimeout(() => btn.style.transform = '', 300);
        if(btn.dataset.cnt >= 3) submitAnswer('no');
    }

    // Анимация сердечек
    function startHearts() {
        const hearts = ['❤️', '💖', '💕', '✨', '🌸'];
        const interval = setInterval(() => {
            const h = document.createElement('div'); h.className = 'heart';
            h.textContent = hearts[Math.floor(Math.random()*hearts.length)];
            h.style.left = Math.random()*100+'vw'; h.style.animationDuration = (2+Math.random()*2)+'s';
            h.style.fontSize = (20+Math.random()*20)+'px';
            document.body.appendChild(h); setTimeout(()=>h.remove(), 3000);
        }, 150);
        setTimeout(()=>clearInterval(interval), 6000);
    }

    // Фон
    const canvas = document.getElementById('bg-canvas');
    const ctx = canvas.getContext('2d');
    let particles = [];
    window.addEventListener('resize', () => { canvas.width = window.innerWidth; canvas.height = window.innerHeight; });
    canvas.width = window.innerWidth; canvas.height = window.innerHeight;
    for(let i=0;i<50;i++) particles.push({x:Math.random()*canvas.width, y:Math.random()*canvas.height, s:Math.random()*2+1, vx:(Math.random()-.5)*.5, vy:(Math.random()-.5)*.5, o:Math.random()*.5+.1});
    (function anim(){ctx.clearRect(0,0,canvas.width,canvas.height); particles.forEach(p=>{ctx.fillStyle=`rgba(139,92,246,${p.o})`;ctx.beginPath();ctx.arc(p.x,p.y,p.s,0,Math.PI*2);ctx.fill();p.x+=p.vx;p.y+=p.vy;if(p.x<0||p.x>canvas.width)p.vx*=-1;if(p.y<0||p.y>canvas.height)p.vy*=-1});requestAnimationFrame(anim)})();
    document.getElementById('firstName')?.focus();
</script>
</body>
</html>
