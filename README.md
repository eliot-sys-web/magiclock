<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", sans-serif;
    overflow: hidden;
    background: #000;
    width: 100vw;
    height: 100vh;
}

/* LOCKSCREEN */
#lockscreen {
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
    background-size: cover;
    background-position: center;
    display: flex;
    flex-direction: column;
    position: relative;
}

#lockscreen.custom {
    background-image: var(--bg-lock);
}

/* STATUS BAR */
.status-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 8px 24px 0;
    height: 48px;
    color: white;
    font-size: 15px;
    font-weight: 600;
}

.status-left {
    display: flex;
    gap: 6px;
    align-items: center;
}

.status-icon {
    font-size: 13px;
}

/* TIME */
.lock-time {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    margin-top: 80px;
}

.time-display {
    font-size: 80px;
    font-weight: 200;
    color: white;
    letter-spacing: -3px;
    line-height: 1;
}

.date-display {
    font-size: 18px;
    font-weight: 600;
    color: white;
    margin-top: 4px;
}

/* NOTIFICATION AREA */
.notification {
    display: none;
    margin: 0 20px;
    padding: 16px;
    background: rgba(255,255,255,0.08);
    backdrop-filter: blur(40px) saturate(180%);
    -webkit-backdrop-filter: blur(40px) saturate(180%);
    border-radius: 20px;
    border: 0.5px solid rgba(255,255,255,0.15);
    color: white;
}

/* CODE AREA */
.code-area {
    margin: auto 0;
    padding: 40px 0;
    display: flex;
    flex-direction: column;
    align-items: center;
}

#message {
    color: white;
    font-size: 15px;
    font-weight: 400;
    margin-bottom: 16px;
    height: 24px;
    text-align: center;
}

#dots {
    display: flex;
    gap: 18px;
    margin-bottom: 24px;
}

.dot {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    background: rgba(255,255,255,0.3);
    transition: all 0.15s ease;
}

.dot.filled {
    background: white;
    transform: scale(1.15);
}

/* KEYBOARD */
.keyboard {
    padding: 0 12px 36px;
}

.kb-row {
    display: flex;
    justify-content: center;
    gap: 30px;
    margin-bottom: 16px;
}

.key {
    width: 75px;
    height: 75px;
    border-radius: 50%;
    background: rgba(255,255,255,0.1);
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    border: 0.5px solid rgba(255,255,255,0.15);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.1s;
    position: relative;
}

.key:active {
    background: rgba(255,255,255,0.25);
    transform: scale(0.92);
}

.key-num {
    font-size: 32px;
    font-weight: 300;
    color: white;
    line-height: 1;
}

.key-text {
    font-size: 9px;
    font-weight: 500;
    color: rgba(255,255,255,0.4);
    letter-spacing: 3px;
    margin-top: 2px;
    text-transform: uppercase;
}

.key-empty {
    opacity: 0;
    pointer-events: none;
}

.key-delete {
    background: rgba(0,0,0,0.2);
    backdrop-filter: blur(20px);
}

.key-delete svg {
    width: 26px;
    height: 26px;
    fill: white;
}

/* BOTTOM BAR */
.bottom-bar {
    display: flex;
    justify-content: space-between;
    padding: 0 40px 28px;
    align-items: center;
}

.bottom-action {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: rgba(255,255,255,0.12);
    backdrop-filter: blur(20px);
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    cursor: pointer;
}

.bottom-action:active {
    background: rgba(255,255,255,0.2);
}

.bottom-text {
    color: white;
    font-size: 13px;
    font-weight: 500;
    margin-top: 6px;
}

.swipe-hint {
    text-align: center;
    color: rgba(255,255,255,0.6);
    font-size: 16px;
    margin-top: 12px;
}

/* HOMESCREEN */
#homescreen {
    width: 100%;
    height: 100%;
    background: linear-gradient(180deg, #667eea 0%, #764ba2 100%);
    background-size: cover;
    background-position: center;
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 200;
}

#homescreen.custom {
    background-image: var(--bg-home);
}

#homescreen.show {
    display: block;
    animation: slideUp 0.35s cubic-bezier(0.36, 0, 0.66, 0);
}

@keyframes slideUp {
    from { transform: translateY(100%); }
    to { transform: translateY(0); }
}

#secret {
    position: absolute;
    bottom: 30px;
    right: 20px;
    color: rgba(255,255,255,0.25);
    font-size: 10px;
    text-align: right;
    line-height: 1.5;
}

/* SETUP */
#setup {
    width: 100vw;
    height: 100vh;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    overflow-y: auto;
}

#setup h1 {
    font-size: 28px;
    color: white;
    margin-bottom: 40px;
    font-weight: 600;
}

.upload-box {
    background: rgba(255,255,255,0.12);
    backdrop-filter: blur(20px);
    border-radius: 20px;
    padding: 24px;
    margin-bottom: 16px;
    width: 100%;
    max-width: 340px;
}

.upload-box h3 {
    color: white;
    font-size: 16px;
    margin-bottom: 12px;
    font-weight: 600;
}

.upload-label {
    display: inline-block;
    background: white;
    color: #667eea;
    padding: 12px 28px;
    border-radius: 12px;
    font-weight: 600;
    font-size: 15px;
    cursor: pointer;
}

.upload-input { display: none; }

.preview {
    margin-top: 12px;
    max-width: 100%;
    max-height: 120px;
    border-radius: 10px;
    display: none;
}

.preview.show { display: block; }

#startBtn {
    background: white;
    color: #667eea;
    border: none;
    padding: 16px 48px;
    border-radius: 14px;
    font-size: 17px;
    font-weight: 600;
    cursor: pointer;
    margin-top: 24px;
    display: none;
}

#startBtn.show { display: inline-block; }

.skip { color: rgba(255,255,255,0.8); margin-top: 16px; cursor: pointer; font-size: 15px; }

/* SETTINGS */
#settingsBtn {
    position: fixed;
    top: 60px;
    left: 20px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(0,0,0,0.25);
    backdrop-filter: blur(20px);
    border: 0.5px solid rgba(255,255,255,0.2);
    color: white;
    font-size: 18px;
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 300;
    cursor: pointer;
}

#settingsBtn.show { display: flex; }

#menu {
    position: fixed;
    top: 110px;
    left: 20px;
    background: rgba(30,30,30,0.95);
    backdrop-filter: blur(40px);
    border-radius: 14px;
    padding: 8px;
    display: none;
    z-index: 301;
    min-width: 200px;
    border: 0.5px solid rgba(255,255,255,0.1);
}

#menu.show { display: block; }

.menu-opt {
    padding: 12px 14px;
    color: white;
    font-size: 15px;
    cursor: pointer;
    border-radius: 8px;
}

.menu-opt:active { background: rgba(255,255,255,0.1); }

@keyframes shake {
    0%, 100% { transform: translateX(0); }
    25% { transform: translateX(-12px); }
    75% { transform: translateX(12px); }
}

.shake { animation: shake 0.35s; }
</style>
</head>
<body>

<!-- SETUP -->
<div id="setup">
    <h1>📱 iPhone Lock Screen</h1>
    
    <div class="upload-box">
        <h3>Fond d'écran verrouillé</h3>
        <label class="upload-label" for="lockBg">Choisir</label>
        <input type="file" id="lockBg" class="upload-input" accept="image/*">
        <img id="prev1" class="preview">
    </div>
    
    <div class="upload-box">
        <h3>Fond d'écran accueil</h3>
        <label class="upload-label" for="homeBg">Choisir</label>
        <input type="file" id="homeBg" class="upload-input" accept="image/*">
        <img id="prev2" class="preview">
    </div>
    
    <button id="startBtn">Démarrer</button>
    <div class="skip" onclick="skip()">Passer</div>
</div>

<!-- LOCKSCREEN -->
<div id="lockscreen" style="display:none;">
    <div class="status-bar">
        <div class="status-left">
            <span class="status-icon">📶</span>
            <span class="status-icon">📡</span>
        </div>
        <div>🔋</div>
    </div>
    
    <div class="lock-time">
        <div class="time-display" id="time">12:00</div>
        <div class="date-display" id="date">lundi 13 janvier</div>
    </div>
    
    <div class="code-area">
        <div id="message">Saisir le code</div>
        <div id="dots">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
    </div>
    
    <div class="keyboard">
        <div class="kb-row">
            <div class="key" data-k="1"><div class="key-num">1</div></div>
            <div class="key" data-k="2"><div class="key-num">2</div><div class="key-text">ABC</div></div>
            <div class="key" data-k="3"><div class="key-num">3</div><div class="key-text">DEF</div></div>
        </div>
        <div class="kb-row">
            <div class="key" data-k="4"><div class="key-num">4</div><div class="key-text">GHI</div></div>
            <div class="key" data-k="5"><div class="key-num">5</div><div class="key-text">JKL</div></div>
            <div class="key" data-k="6"><div class="key-num">6</div><div class="key-text">MNO</div></div>
        </div>
        <div class="kb-row">
            <div class="key" data-k="7"><div class="key-num">7</div><div class="key-text">PQRS</div></div>
            <div class="key" data-k="8"><div class="key-num">8</div><div class="key-text">TUV</div></div>
            <div class="key" data-k="9"><div class="key-num">9</div><div class="key-text">WXYZ</div></div>
        </div>
        <div class="kb-row">
            <div class="key key-empty"></div>
            <div class="key" data-k="0"><div class="key-num">0</div></div>
            <div class="key key-delete" onclick="del()">
                <svg viewBox="0 0 24 24"><path d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm-3 12.59L17.59 17 14 13.41 10.41 17 9 15.59 12.59 12 9 8.41 10.41 7 14 10.59 17.59 7 19 8.41 15.41 12 19 15.59z"/></svg>
            </div>
        </div>
    </div>
</div>

<!-- HOMESCREEN -->
<div id="homescreen">
    <div id="secret"></div>
</div>

<button id="settingsBtn" onclick="menu()">⚙️</button>
<div id="menu">
    <div class="menu-opt" onclick="change()">Changer les fonds</div>
    <div class="menu-opt" onclick="location.reload()">Recommencer</div>
</div>

<script>
const CODES = {"123456":"Brad Pitt","654321":"Emma Watson","111111":"Leonardo DiCaprio"};
let inp = "", first = null, unlock = false;
let lockBg = null, homeBg = null;

const setup = document.getElementById('setup');
const lock = document.getElementById('lockscreen');
const home = document.getElementById('homescreen');
const dots = document.querySelectorAll('.dot');
const msg = document.getElementById('message');
const btn = document.getElementById('settingsBtn');

function updateTime() {
    const now = new Date();
    const h = String(now.getHours()).padStart(2,'0');
    const m = String(now.getMinutes()).padStart(2,'0');
    document.getElementById('time').textContent = `${h}:${m}`;
    
    const days = ['dimanche','lundi','mardi','mercredi','jeudi','vendredi','samedi'];
    const months = ['janvier','février','mars','avril','mai','juin','juillet','août','septembre','octobre','novembre','décembre'];
    document.getElementById('date').textContent = `${days[now.getDay()]} ${now.getDate()} ${months[now.getMonth()]}`;
}
setInterval(updateTime, 1000);
updateTime();

document.getElementById('lockBg').onchange = e => {
    const f = e.target.files[0];
    if(f) {
        const r = new FileReader();
        r.onload = ev => {
            lockBg = ev.target.result;
            document.getElementById('prev1').src = lockBg;
            document.getElementById('prev1').classList.add('show');
            check();
        };
        r.readAsDataURL(f);
    }
};

document.getElementById('homeBg').onchange = e => {
    const f = e.target.files[0];
    if(f) {
        const r = new FileReader();
        r.onload = ev => {
            homeBg = ev.target.result;
            document.getElementById('prev2').src = homeBg;
            document.getElementById('prev2').classList.add('show');
            check();
        };
        r.readAsDataURL(f);
    }
};

function check() {
    if(lockBg && homeBg) document.getElementById('startBtn').classList.add('show');
}

document.getElementById('startBtn').onclick = start;
function skip() { start(); }

function start() {
    if(lockBg) {
        lock.style.setProperty('--bg-lock', `url(${lockBg})`);
        lock.classList.add('custom');
    }
    if(homeBg) {
        home.style.setProperty('--bg-home', `url(${homeBg})`);
        home.classList.add('custom');
    }
    setup.style.display = 'none';
    lock.style.display = 'flex';
    btn.classList.add('show');
}

document.querySelectorAll('.key[data-k]').forEach(k => {
    k.onclick = () => { if(!unlock) handle(k.dataset.k); };
});

function handle(d) {
    if(inp.length >= 6) return;
    inp += d;
    updateDots();
    if(navigator.vibrate) navigator.vibrate(10);
    
    if(inp.length === 6) {
        setTimeout(() => {
            if(!first) {
                first = inp;
                msg.textContent = "Saisir à nouveau le code";
                setTimeout(() => reset(), 600);
            } else if(CODES[inp]) {
                msg.textContent = "";
                unlockScreen(CODES[inp]);
            } else {
                msg.textContent = "Code incorrect";
                lock.classList.add('shake');
                setTimeout(() => {
                    lock.classList.remove('shake');
                    msg.textContent = "Saisir à nouveau le code";
                    reset();
                }, 400);
            }
        }, 300);
    }
}

function del() {
    if(inp.length > 0) {
        inp = inp.slice(0, -1);
        updateDots();
        if(navigator.vibrate) navigator.vibrate(10);
    }
}

function updateDots() {
    dots.forEach((d, i) => {
        if(i < inp.length) d.classList.add('filled');
        else d.classList.remove('filled');
    });
}

function reset() {
    inp = "";
    updateDots();
}

function unlockScreen(celeb) {
    unlock = true;
    document.getElementById('secret').innerHTML = `${celeb}<br>${first}`;
    home.style.display = 'block';
    home.classList.add('show');
    setTimeout(() => { lock.style.display = 'none'; }, 350);
    if(navigator.vibrate) navigator.vibrate([50,100,50]);
}

function menu() {
    document.getElementById('menu').classList.toggle('show');
}

function change() {
    unlock = false;
    first = null;
    inp = "";
    msg.textContent = "Saisir le code";
    home.classList.remove('show');
    home.style.display = 'none';
    lock.style.display = 'none';
    setup.style.display = 'flex';
    btn.classList.remove('show');
    document.getElementById('menu').classList.remove('show');
}

document.addEventListener('click', e => {
    if(!e.target.closest('#settingsBtn') && !e.target.closest('#menu')) {
        document.getElementById('menu').classList.remove('show');
    }
});
</script>
</body>
</html>
