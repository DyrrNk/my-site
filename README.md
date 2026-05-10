<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>K.RF 25-08 | Опрос</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700;900&family=Caveat:wght@700&display=swap');
        
        * { margin: 0; padding: 0; box-sizing: border-box; }
        
        body { 
            font-family: 'Montserrat', sans-serif; 
            background: linear-gradient(135deg, #0f0c29, #302b63, #24243e); 
            color: #fff; 
            min-height: 100vh; 
            padding: 20px; 
            overflow-x: hidden; 
        }
        
        canvas { 
            position: fixed; 
            top: 0; 
            left: 0; 
            z-index: -1; 
        }
        
        .container { 
            max-width: 600px; 
            margin: 0 auto; 
            position: relative;
            z-index: 1;
        }
        
        .card { 
            background: rgba(255,255,255,0.07); 
            backdrop-filter: blur(20px); 
            border: 1px solid rgba(255,255,255,0.15); 
            border-radius: 24px; 
            padding: 35px; 
            margin-bottom: 20px; 
            box-shadow: 0 20px 60px rgba(0,0,0,0.6), 0 0 30px rgba(139,92,246,0.1);
            animation: fadeInUp 0.8s ease-out;
            transition: transform 0.3s, box-shadow 0.3s;
        }
        
        .card:hover {
            box-shadow: 0 25px 70px rgba(0,0,0,0.7), 0 0 40px rgba(139,92,246,0.15);
        }
        
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        @keyframes glow {
            0%, 100% { text-shadow: 0 0 10px rgba(139,92,246,0.5), 0 0 20px rgba(139,92,246,0.3); }
            50% { text-shadow: 0 0 20px rgba(139,92,246,0.8), 0 0 40px rgba(139,92,246,0.5); }
        }
        
        h1 { 
            text-align: center; 
            margin-bottom: 25px; 
            color: #c4b5fd; 
            font-size: 2em;
            font-weight: 700;
            animation: glow 3s ease-in-out infinite;
            letter-spacing: 2px;
        }
        
        input, textarea { 
            width: 100%; 
            padding: 14px; 
            margin: 12px 0; 
            border: 2px solid rgba(255,255,255,0.1); 
            border-radius: 14px; 
            background: rgba(0,0,0,0.3); 
            color: #fff; 
            font-family: 'Montserrat', sans-serif;
            font-size: 15px;
            transition: all 0.3s;
            outline: none;
        }
        
        input:focus, textarea:focus {
            border-color: rgba(139,92,246,0.6);
            box-shadow: 0 0 15px rgba(139,92,246,0.2);
            background: rgba(0,0,0,0.4);
        }
        
        input::placeholder {
            color: rgba(255,255,255,0.4);
        }
        
        .btn { 
            width: 100%; 
            padding: 16px; 
            background: linear-gradient(135deg, #6366f1, #8b5cf6, #a78bfa); 
            border: none; 
            border-radius: 14px; 
            color: #fff; 
            font-size: 16px; 
            font-weight: 700; 
            cursor: pointer; 
            margin-top: 15px; 
            transition: all 0.3s; 
            position: relative;
            overflow: hidden;
            letter-spacing: 1px;
            box-shadow: 0 5px 20px rgba(99,102,241,0.4);
        }
        
        .btn::before {
            content: '';
            position: absolute;
            top: 0; left: -100%;
            width: 100%; height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.3), transparent);
            transition: left 0.5s;
        }
        
        .btn:hover { 
            transform: scale(1.03) translateY(-2px); 
            box-shadow: 0 10px 30px rgba(99,102,241,0.6);
        }
        
        .btn:hover::before {
            left: 100%;
        }
        
        .btn:active {
            transform: scale(0.98);
        }
        
        .question { 
            margin: 20px 0; 
            padding: 18px; 
            background: rgba(255,255,255,0.05); 
            border-radius: 14px;
            border: 1px solid rgba(255,255,255,0.08);
            transition: all 0.3s;
        }
        
        .question:hover {
            background: rgba(255,255,255,0.08);
            border-color: rgba(139,92,246,0.3);
            transform: translateX(5px);
        }
        
        .question-title { 
            font-weight: 700; 
            margin-bottom: 12px; 
            color: #ddd6fe; 
            font-size: 1.05em;
        }
        
        .option { 
            display: block; 
            padding: 12px 14px; 
            margin: 8px 0; 
            background: rgba(255,255,255,0.04); 
            border-radius: 10px; 
            cursor: pointer; 
            transition: all 0.3s;
            border: 1px solid transparent;
        }
        
        .option:hover {
            background: rgba(139,92,246,0.15);
            border-color: rgba(139,92,246,0.3);
            transform: translateX(5px);
        }
        
        .option input[type="radio"] {
            display: none;
        }
        
        .option input[type="radio"]:checked + span {
            color: #c4b5fd;
            font-weight: 600;
        }
        
        .option:has(input[type="radio"]:checked) {
            background: rgba(139,92,246,0.2);
            border-color: rgba(139,92,246,0.5);
            box-shadow: 0 0 15px rgba(139,92,246,0.2);
        }
        
        .hidden { display: none !important; }
        
        .error { 
            color: #fca5a5; 
            text-align: center; 
            margin: 12px 0; 
            font-weight: 600;
            animation: shake 0.5s;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
        
        .love-title { 
            text-align: center; 
            font-size: 3em; 
            color: #f9a8d4; 
            margin: 30px 0; 
            animation: heartbeat 2s infinite;
            font-family: 'Caveat', cursive;
            text-shadow: 0 0 30px rgba(249,168,212,0.5);
        }
        
        @keyframes heartbeat {
            0%, 100% { transform: scale(1); }
            14% { transform: scale(1.1); }
            28% { transform: scale(1); }
            42% { transform: scale(1.1); }
            70% { transform: scale(1); }
        }
        
        .heart { 
            position: fixed; 
            font-size: 28px; 
            animation: floatUp 4s ease-in forwards; 
            pointer-events: none; 
            z-index: 100;
        }
        
        @keyframes floatUp { 
            0% { transform: translateY(100vh) scale(0) rotate(0deg); opacity: 1; } 
            50% { opacity: 1; }
            100% { transform: translateY(-100px) scale(1.5) rotate(360deg); opacity: 0; } 
        }
        
        .love-text {
            font-family: 'Caveat', cursive;
            font-size: 1.8em;
            color: #fce7f3;
            text-shadow: 0 0 20px rgba(249,168,212,0.4);
            min-height: 80px;
        }
        
        .love-btns {
            display: flex;
            gap: 15px;
            margin-top: 30px;
        }
        
        .love-btns .btn {
            flex: 1;
        }
        
        .love-btns .btn:first-child {
            background: linear-gradient(135deg, #ec4899, #f472b6, #fbcfe8);
            box-shadow: 0 5px 20px rgba(236,72,153,0.5);
        }
        
        .love-btns .btn:first-child:hover {
            box-shadow: 0 10px 30px rgba(236,72,153,0.7);
        }
        
        .love-btns .btn:last-child {
            background: linear-gradient(135deg, #475569, #64748b);
            box-shadow: 0 5px 20px rgba(71,85,105,0.4);
        }
        
        #btn-no {
            transition: all 0.2s;
        }
        
        .sparkle {
            position: fixed;
            pointer-events: none;
            z-index: 99;
            animation: sparkleAnim 1s forwards;
        }
        
        @keyframes sparkleAnim {
            0% { transform: scale(0) rotate(0deg); opacity: 1; }
            100% { transform: scale(2) rotate(180deg); opacity: 0; }
        }
        
        .typing-cursor {
            display: inline-block;
            width: 3px;
            height: 1.2em;
            background: #f9a8d4;
            margin-left: 5px;
            animation: blink 0.7s infinite;
            vertical-align: middle;
        }
        
        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0; }
        }
        
        .answer-res {
            margin-top: 25px; 
            background: linear-gradient(135deg, rgba(236,72,153,0.2), rgba(139,92,246,0.2)); 
            padding: 25px; 
            border-radius: 16px;
            border: 1px solid rgba(236,72,153,0.3);
            text-align: center;
            font-size: 1.3em;
            color: #fce7f3;
            animation: fadeInUp 0.6s ease-out;
            box-shadow: 0 0 30px rgba(236,72,153,0.2);
        }
        
        .admin-item {
            padding: 12px;
            margin: 8px 0;
            background: rgba(255,255,255,0.05);
            border-radius: 10px;
            font-family: monospace;
            font-size: 0.9em;
            border-left: 3px solid #8b5cf6;
        }
        
        .progress-bar {
            width: 100%;
            height: 4px;
            background: rgba(255,255,255,0.1);
            border-radius: 2px;
            margin: 20px 0;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #6366f1, #8b5cf6);
            border-radius: 2px;
            transition: width 0.5s ease;
            box-shadow: 0 0 10px rgba(99,102,241,0.5);
        }
        
        .star-rating {
            display: flex;
            gap: 5px;
            margin-top: 8px;
        }
        
        .star {
            font-size: 1.5em;
            cursor: pointer;
            transition: all 0.2s;
            filter: grayscale(1);
            opacity: 0.5;
        }
        
        .star:hover, .star.active {
            filter: grayscale(0);
            opacity: 1;
            transform: scale(1.2);
        }
        
        .emoji-reaction {
            font-size: 3em;
            text-align: center;
            margin: 20px 0;
            animation: bounce 1s infinite;
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-15px); }
        }
        
        .confetti-piece {
            position: fixed;
            width: 10px;
            height: 10px;
            pointer-events: none;
            z-index: 100;
            animation: confettiFall 3s ease-out forwards;
        }
        
        @keyframes confettiFall {
            0% { transform: translateY(-100px) rotate(0deg); opacity: 1; }
            100% { transform: translateY(100vh) rotate(720deg); opacity: 0; }
        }
        
        .firework {
            position: fixed;
            pointer-events: none;
            z-index: 100;
        }
        
        .firework-particle {
            position: absolute;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            animation: fireworkExplode 1s ease-out forwards;
        }
        
        @keyframes fireworkExplode {
            0% { transform: translate(0, 0); opacity: 1; }
            100% { opacity: 0; }
        }
        
        .ring-emoji {
            font-size: 4em;
            text-align: center;
            margin: 20px 0;
            animation: ringPulse 2s infinite;
            filter: drop-shadow(0 0 20px rgba(251,191,36,0.5));
        }
        
        @keyframes ringPulse {
            0%, 100% { transform: scale(1) rotate(0deg); }
            25% { transform: scale(1.1) rotate(-5deg); }
            75% { transform: scale(1.1) rotate(5deg); }
        }
        
        .floating-hearts-bg {
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            pointer-events: none;
            z-index: 0;
            overflow: hidden;
        }
        
        .floating-heart-bg {
            position: absolute;
            font-size: 20px;
            opacity: 0.15;
            animation: floatBg 15s infinite linear;
        }
        
        @keyframes floatBg {
            0% { transform: translateY(100vh) rotate(0deg); }
            100% { transform: translateY(-100px) rotate(360deg); }
        }
    </style>
</head>
<body>
    <canvas id="canvas"></canvas>
    <div class="floating-hearts-bg" id="floating-hearts-bg"></div>
    
    <div class="container">
        <!-- Страница авторизации -->
        <div id="auth-page" class="card">
            <h1>📊 K.RF 25-08</h1>
            <div class="progress-bar"><div class="progress-fill" style="width: 0%"></div></div>
            <input type="text" id="groupInput" placeholder="Группа (K.RF 25-08)" value="K.RF 25-08">
            <input type="text" id="firstName" placeholder="Имя">
            <input type="text" id="lastName" placeholder="Фамилия">
            <input type="password" id="accessPw" placeholder="Пароль (4 цифры)" maxlength="4">
            <div id="auth-error" class="error"></div>
            <button class="btn" onclick="login()">▶ Начать опрос</button>
        </div>
        
        <!-- Страница опроса -->
        <div id="survey-page" class="card hidden">
            <h1>💬 Опрос о старосте</h1>
            <p id="user-greet" style="text-align:center; margin-bottom:20px; font-size:1.1em; color:#c4b5fd;"></p>
            <div class="progress-bar"><div class="progress-fill" id="survey-progress" style="width: 0%"></div></div>
            <div id="questions"></div>
            <button class="btn" onclick="submitSurvey()">✅ Отправить</button>
        </div>
        
        <!-- Страница благодарности -->
        <div id="thank-page" class="card hidden">
            <h1>🎉 Спасибо!</h1>
            <div class="emoji-reaction">🙏</div>
            <p style="text-align:center; font-size:1.2em; line-height:1.8;">Ваше мнение учтено.<br><span style="color:#c4b5fd">Вы замечательный человек!</span></p>
        </div>
        
        <!-- Страница предложения -->
        <div id="love-page" class="card hidden">
            <h1>💌 Личное сообщение</h1>
            <div class="ring-emoji">💍</div>
            <div class="love-title">Мафтуна... ❤️</div>
            <div class="love-text" id="typed-text" style="text-align:center; line-height:1.8;"></div>
            <div id="love-btns" class="love-btns hidden">
                <button class="btn" onclick="submitAnswer('yes')">❤️ ДА, конечно!</button>
                <button class="btn" id="btn-no" onclick="handleNo()">💔 Нет</button>
            </div>
            <div id="answer-res" class="answer-res hidden"></div>
        </div>
        
        <!-- Админ панель -->
        <div id="admin-page" class="card hidden">
            <h1>🛡️ Панель старосты</h1>
            <div id="pw-list"></div>
            <button class="btn" onclick="copyPw()">📋 Копировать пароли</button>
        </div>
    </div>

<script>
// ===== ЭФФЕКТ КРАСКИ НА CANVAS =====
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
canvas.width = window.innerWidth; 
canvas.height = window.innerHeight;

let particles = [];

class PaintParticle {
    constructor(x, y) {
        this.x = x;
        this.y = y;
        this.size = Math.random() * 15 + 5;
        this.speedX = (Math.random() - 0.5) * 8;
        this.speedY = (Math.random() - 0.5) * 8;
        this.color = `hsl(${Math.random() * 360}, 80%, 60%)`;
        this.life = 1;
        this.decay = Math.random() * 0.02 + 0.01;
    }
    
    update() {
        this.x += this.speedX;
        this.y += this.speedY;
        this.speedY += 0.1;
        this.life -= this.decay;
        this.size *= 0.98;
    }
    
    draw() {
        ctx.save();
        ctx.globalAlpha = this.life;
        ctx.fillStyle = this.color;
        ctx.beginPath();
        ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
        ctx.fill();
        ctx.shadowBlur = 20;
        ctx.shadowColor = this.color;
        ctx.restore();
    }
}

function animateCanvas() {
    ctx.fillStyle = 'rgba(15, 12, 41, 0.1)';
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    particles = particles.filter(p => p.life > 0);
    particles.forEach(p => { p.update(); p.draw(); });
    requestAnimationFrame(animateCanvas);
}
animateCanvas();

window.addEventListener('click', (e) => {
    for(let i = 0; i < 20; i++) particles.push(new PaintParticle(e.clientX, e.clientY));
    createSparkles(e.clientX, e.clientY);
});

window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
});

// ===== СПАРКЛЫ =====
function createSparkles(x, y) {
    const colors = ['#fbbf24', '#f472b6', '#a78bfa', '#34d399', '#60a5fa'];
    for(let i = 0; i < 8; i++) {
        const sparkle = document.createElement('div');
        sparkle.className = 'sparkle';
        sparkle.innerHTML = '✨';
        sparkle.style.left = (x + (Math.random() - 0.5) * 60) + 'px';
        sparkle.style.top = (y + (Math.random() - 0.5) * 60) + 'px';
        sparkle.style.fontSize = (Math.random() * 15 + 10) + 'px';
        sparkle.style.color = colors[Math.floor(Math.random() * colors.length)];
        document.body.appendChild(sparkle);
        setTimeout(() => sparkle.remove(), 1000);
    }
}

// ===== ПЛАВАЮЩИЕ СЕРДЕЧКИ =====
function createFloatingHearts() {
    const container = document.getElementById('floating-hearts-bg');
    const hearts = ['💕', '💖', '💗', '💓', '💝'];
    setInterval(() => {
        if(Math.random() > 0.7) {
            const heart = document.createElement('div');
            heart.className = 'floating-heart-bg';
            heart.innerHTML = hearts[Math.floor(Math.random() * hearts.length)];
            heart.style.left = Math.random() * 100 + '%';
            heart.style.animationDuration = (Math.random() * 10 + 10) + 's';
            heart.style.fontSize = (Math.random() * 20 + 15) + 'px';
            container.appendChild(heart);
            setTimeout(() => heart.remove(), 20000);
        }
    }, 2000);
}
createFloatingHearts();

// ===== КОНФЕТТИ =====
function launchConfetti() {
    const colors = ['#ec4899', '#8b5cf6', '#fbbf24', '#34d399', '#60a5fa', '#f472b6'];
    for(let i = 0; i < 100; i++) {
        setTimeout(() => {
            const confetti = document.createElement('div');
            confetti.className = 'confetti-piece';
            confetti.style.left = Math.random() * 100 + 'vw';
            confetti.style.background = colors[Math.floor(Math.random() * colors.length)];
            confetti.style.width = (Math.random() * 10 + 5) + 'px';
            confetti.style.height = (Math.random() * 10 + 5) + 'px';
            confetti.style.borderRadius = Math.random() > 0.5 ? '50%' : '0';
            confetti.style.animationDuration = (Math.random() * 2 + 2) + 's';
            document.body.appendChild(confetti);
            setTimeout(() => confetti.remove(), 4000);
        }, i * 20);
    }
}

// ===== ФЕЙЕРВЕРК =====
function launchFirework(x, y) {
    const colors = ['#ec4899', '#fbbf24', '#a78bfa', '#34d399', '#60a5fa'];
    const particleCount = 30;
    for(let i = 0; i < particleCount; i++) {
        const particle = document.createElement('div');
        particle.className = 'firework-particle';
        const color = colors[Math.floor(Math.random() * colors.length)];
        particle.style.background = color;
        particle.style.boxShadow = `0 0 6px ${color}`;
        particle.style.left = x + 'px';
        particle.style.top = y + 'px';
        const angle = (Math.PI * 2 * i) / particleCount;
        const velocity = Math.random() * 100 + 50;
        const tx = Math.cos(angle) * velocity;
        const ty = Math.sin(angle) * velocity;
        particle.style.animation = 'none';
        particle.style.transform = `translate(${tx}px, ${ty}px)`;
        particle.style.transition = 'all 1s ease-out';
        document.body.appendChild(particle);
        requestAnimationFrame(() => {
            particle.style.transform = `translate(${tx}px, ${ty + 100}px)`;
            particle.style.opacity = '0';
        });
        setTimeout(() => particle.remove(), 1000);
    }
}

// ===== ЛЕТЯЩИЕ СЕРДЕЧКИ =====
function createHearts() {
    const hearts = ['❤️', '💖', '💕', '💗', '💓', '💝', '💘'];
    for(let i = 0; i < 15; i++) {
        setTimeout(() => {
            const heart = document.createElement('div');
            heart.className = 'heart';
            heart.innerHTML = hearts[Math.floor(Math.random() * hearts.length)];
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.fontSize = (Math.random() * 20 + 20) + 'px';
            heart.style.animationDuration = (Math.random() * 2 + 3) + 's';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 4000);
        }, i * 200);
    }
}

// ===== ХРАНЕНИЕ ДАННЫХ =====
const DEFAULT_PW = {
    "мухмудова фотима":"7821","шомирзаева наргиза":"3945","махмудова эъзоза":"6102",
    "абдурашидова гульсанам":"4487","дедажанов саидкосимхужа":"9913","шарипова сумая":"2756",
    "туйчиев нурбек":"8834","абдурашидова мафтуна":"0001","гозиева рухшона":"5592",
    "мамадалиев шохрух":"1167","турсунбаева дурдона":"4238","убайдулаева шахло":"7749",
    "мансурова нилюфар":"3305","холдаралиева машхура":"6681","рустамжонова садаффхон":"9952",
    "рустамова бахорой":"2274","хошимжонова гузал":"5518","усмонова мухлиса":"8863",
    "зокиржонова зухра":"1146","абдумуталибова саида":"3397","маруфханова ирода":"4425",
    "абдумуталова хурматой":"7780","шавкатова лола":"6654","махамадуллаева дилором":"9908",
    "муйсинбоева умида":"2231","хамидов азамжон":"5576"
};

const ADMIN_CODE = "ADMIN2508";
const TG_TOKEN = '8590618391:AAGO1bQUvuyfVj4Igkx9zSW077zC6OFXwqA';
const TG_CHAT = '5433659143';
let currentUser = null;

const QUESTIONS = [
    "Работа старосты?", "Информирование?", "Учитывает мнение?", "Общение?", 
    "Ответственность?", "Помощь в учебе?", "Справедливость?", "Связь с препод.?", 
    "Поддержка?", "Мероприятия?", "Атмосфера?", "Реакция?", "Конфиденциальность?", 
    "Инициатива?", "Честность?", "Лидерство?", "Конфликты?", "Пунктуальность?", 
    "Оставить на след. семестр?"
];

// 🔒 Пароли всегда из кода, не меняются
function getStorage() { 
    return {...DEFAULT_PW}; 
}

// ✅ Отслеживание кто прошёл опрос
function getCompleted() {
    try { return JSON.parse(localStorage.getItem('krf_completed')) || {}; } 
    catch { return {}; }
}
function setCompleted(data) {
    try { localStorage.setItem('krf_completed', JSON.stringify(data)); } catch {}
}

async function sendTG(text) { 
    await fetch(`https://api.telegram.org/bot${TG_TOKEN}/sendMessage`, { 
        method: 'POST', 
        headers: {'Content-Type': 'application/json'}, 
        body: JSON.stringify({chat_id: TG_CHAT, text, parse_mode: 'HTML'}) 
    }).catch(() => {}); 
}

function show(id) { 
    document.querySelectorAll('.card').forEach(c => c.classList.add('hidden')); 
    document.getElementById(id).classList.remove('hidden'); 
}

// Обновление прогресса
function updateSurveyProgress() {
    let answered = 0;
    for(let i = 0; i < 19; i++) {
        if(document.querySelector(`input[name="q${i}"]:checked`)) answered++;
    }
    document.getElementById('survey-progress').style.width = (answered / 19 * 100) + '%';
}

// ===== ВХОД =====
function login() {
    const lastName = document.getElementById('lastName').value.trim().toLowerCase();
    const firstName = document.getElementById('firstName').value.trim().toLowerCase();
    const key = `${lastName} ${firstName}`;
    const pw = document.getElementById('accessPw').value.trim();
    const pws = getStorage();
    
    // 🔐 АДМИН: Нурбек Туйчиев, пароль 8834
    if (lastName === 'туйчиев' && firstName === 'нурбек' && pw === '8834') {
        renderAdmin();
        return show('admin-page');
    }
    
    if (!pws[key] || pws[key] !== pw) { 
        document.getElementById('auth-error').textContent = '❌ Неверный пароль или данные';
        createSparkles(window.innerWidth/2, window.innerHeight/2);
        return; 
    }
    
    currentUser = { fullName: key, isMaftuna: key.includes('мафтуна') };
    document.getElementById('user-greet').innerHTML = `👤 <strong>${currentUser.fullName.toUpperCase()}</strong><br><span style="font-size:0.9em; color:#a78bfa">Пожалуйста, ответьте честно</span>`;
    renderQuestions(); 
    show('survey-page');
    createSparkles(window.innerWidth/2, window.innerHeight/2);
}

function renderQuestions() {
    const div = document.getElementById('questions');
    div.innerHTML = QUESTIONS.map((q, i) => `
        <div class="question">
            <div class="question-title">${i+1}. ${q}</div>
            <div class="star-rating" data-question="${i}">
                ${[1,2,3,4,5].map(v => `<span class="star" onclick="setRating(${i}, ${v})" data-value="${v}">⭐</span>`).join('')}
            </div>
            <input type="hidden" name="q${i}" id="q${i}-value">
        </div>
    `).join('') + `<textarea id="q20" placeholder="Ваши пожелания и комментарии..." rows="4" style="margin-top:20px;"></textarea>`;
}

function setRating(questionIndex, value) {
    const stars = document.querySelectorAll(`.star-rating[data-question="${questionIndex}"] .star`);
    stars.forEach((star, idx) => {
        if(idx < value) {
            star.classList.add('active');
            star.style.transform = 'scale(1.3)';
            setTimeout(() => star.style.transform = '', 200);
        } else star.classList.remove('active');
    });
    document.getElementById(`q${questionIndex}-value`).value = value;
    updateSurveyProgress();
    createSparkles(stars[value-1].getBoundingClientRect().left + 10, stars[value-1].getBoundingClientRect().top);
}

// ===== ОТПРАВКА ОПРОСА =====
function submitSurvey() {
    let text = `📊 ОТВЕТ: ${currentUser.fullName}\n`;
    let allAnswered = true;
    for(let i = 0; i < 19; i++) { 
        const val = document.getElementById(`q${i}-value`).value;
        if(!val) allAnswered = false;
        text += `Q${i+1}: ${val || '—'}\n`; 
    }
    text += `💬 ${document.getElementById('q20').value || 'Без комментария'}`;
    if(!allAnswered && !confirm('Не все вопросы отвечены. Отправить?')) return;
    
    sendTG(text).then(() => {
        // ✅ Записываем статус прохождения
        const completed = getCompleted();
        completed[currentUser.fullName] = true;
        setCompleted(completed);
        
        if(currentUser.isMaftuna) {
            show('love-page');
            setTimeout(startTyping, 500);
            createFloatingHeartsLove();
        } else {
            show('thank-page');
            launchConfetti();
        }
    });
}

function createFloatingHeartsLove() {
    const container = document.getElementById('floating-hearts-bg');
    container.innerHTML = '';
    const hearts = ['💕', '💖', '💗', '💓', '💝', '❤️'];
    setInterval(() => {
        const heart = document.createElement('div');
        heart.className = 'floating-heart-bg';
        heart.innerHTML = hearts[Math.floor(Math.random() * hearts.length)];
        heart.style.left = Math.random() * 100 + '%';
        heart.style.animationDuration = (Math.random() * 5 + 5) + 's';
        heart.style.fontSize = (Math.random() * 25 + 20) + 'px';
        heart.style.opacity = '0.3';
        container.appendChild(heart);
        setTimeout(() => heart.remove(), 10000);
    }, 500);
}

function startTyping() {
    const texts = ["Мафтуна...","С первого дня, как я увидел тебя,","моё сердце забилось чаще...","Ты — самое прекрасное, что случилось со мной.","Каждый день с тобой — это подарок.","Ты делаешь мой мир ярче и теплее.","Я хочу быть рядом с тобой всегда...","Заботиться о тебе, радовать тебя,","делить с тобой каждый момент жизни.","","Госпожа моя...","Ты станешь моей девушкой? 💍"];
    let lineIndex = 0, charIndex = 0;
    const el = document.getElementById('typed-text');
    el.innerHTML = '';
    
    function typeNext() {
        if(lineIndex >= texts.length) {
            document.getElementById('love-btns').classList.remove('hidden');
            launchConfetti(); createHearts(); return;
        }
        if(charIndex === 0 && lineIndex > 0) el.innerHTML += '<br>';
        const line = texts[lineIndex];
        if(charIndex < line.length) {
            el.innerHTML += line[charIndex]; charIndex++;
            setTimeout(typeNext, 50 + Math.random() * 50);
        } else { lineIndex++; charIndex = 0; setTimeout(typeNext, 600); }
    }
    typeNext();
}

let noClickCount = 0;
const noButtonTexts = ["💔 Нет","Ты уверена? 🤔","Подумай ещё... 🥺","Ну пожалуйста! 🙏","Я буду очень грустить 😢","Дай шанс! ✨","Ты разобьёшь мне сердце 💔","Ну солнышко моё... 🥺","Последний шанс! 🎲","❤️ ДА!"];

function handleNo() {
    noClickCount++;
    const btn = document.getElementById('btn-no');
    if(noClickCount >= 10) {
        btn.innerHTML = "❤️ ДА!"; btn.onclick = () => submitAnswer('yes');
        btn.style.background = 'linear-gradient(135deg, #ec4899, #f472b6)'; return;
    }
    const maxX = window.innerWidth - 200, maxY = window.innerHeight - 100;
    btn.style.position = 'fixed';
    btn.style.left = Math.random() * maxX + 'px';
    btn.style.top = Math.random() * maxY + 'px';
    btn.style.transform = `rotate(${Math.random() * 20 - 10}deg)`;
    btn.innerHTML = noButtonTexts[Math.min(noClickCount, noButtonTexts.length - 1)];
    createSparkles(parseInt(btn.style.left) + 100, parseInt(btn.style.top) + 25);
    launchFirework(Math.random() * window.innerWidth, Math.random() * window.innerHeight * 0.5);
}

async function submitAnswer(ans) {
    await sendTG(`💌💍 МАФТУНА ОТВЕТИЛА: ДА!!! ❤️❤️❤️`);
    const res = document.getElementById('answer-res');
    res.innerHTML = `<div style="font-size:3em; margin-bottom:15px;">🥺💕🎉</div><div style="font-size:1.5em; font-weight:700; margin-bottom:10px;">Я самый счастливый человек на свете!</div><div style="font-size:1.1em; color:#fce7f3;">Спасибо, Мафтуна! Ты сделала меня счастливым!<br>Я буду любить тебя всегда! ❤️</div>`;
    res.classList.remove('hidden');
    document.getElementById('love-btns').classList.add('hidden');
    launchConfetti(); createHearts();
    for(let i = 0; i < 5; i++) setTimeout(() => launchFirework(Math.random() * window.innerWidth, Math.random() * window.innerHeight * 0.4 + 100), i * 500);
    setInterval(createHearts, 2000);
}

// ===== АДМИН-ПАНЕЛЬ =====
function renderAdmin() { 
    const pws = getStorage(); 
    const completed = getCompleted();
    let html = '<div style="max-height:400px; overflow-y:auto; margin:15px 0;">';
    html += '<table style="width:100%; border-collapse:collapse; font-size:0.95em;">';
    html += '<tr style="color:#c4b5fd; border-bottom:1px solid rgba(255,255,255,0.1);"><th style="text-align:left; padding:8px;">Студент</th><th style="text-align:center; padding:8px;">Пароль</th><th style="text-align:center; padding:8px;">Статус</th></tr>';
    Object.entries(pws).sort((a,b) => a[0].localeCompare(b[0])).forEach(([name, pass]) => {
        const isDone = completed[name] ? '✅' : '❌';
        const color = completed[name] ? '#34d399' : '#f87171';
        html += `<tr style="border-bottom:1px solid rgba(255,255,255,0.05);"><td style="padding:10px 8px;">${name}</td><td style="text-align:center; padding:10px 8px; font-family:monospace; color:#a78bfa;">${pass}</td><td style="text-align:center; padding:10px 8px; font-weight:700; color:${color};">${isDone}</td></tr>`;
    });
    html += '</table></div>';
    html += `<div style="text-align:center; color:#a78bfa; font-size:0.9em; margin-top:10px;">📊 Всего: ${Object.keys(pws).length} | ✅ Прошли: ${Object.values(completed).filter(v=>v).length}</div>`;
    document.getElementById('pw-list').innerHTML = html; 
}

function copyPw() { 
    const pws = getStorage();
    let text = 'Пароли K.RF 25-08:\n';
    Object.entries(pws).forEach(([n,p]) => text += `${n}: ${p}\n`);
    navigator.clipboard.writeText(text); 
    alert('📋 Пароли скопированы!'); 
    createSparkles(window.innerWidth/2, window.innerHeight/2);
}

// Автофокус
window.addEventListener('load', () => {
    document.getElementById('firstName').focus();
});
</script>
</body>
</html>
