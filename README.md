<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content=" ">
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", sans-serif;
    overflow: hidden;
    background: #000;
}

/* ÉCRAN DE VERROUILLAGE */
#lockscreen {
    width: 100vw;
    height: 100vh;
    background: linear-gradient(135deg, #1e3c72 0%, #2a5298 50%, #7e8ba3 100%);
    background-size: cover;
    background-position: center;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-between;
    padding: 60px 0 40px;
    position: relative;
    overflow: hidden;
}

#lockscreen.custom-bg {
    background-image: var(--bg-image);
}

/* HEURE ET DATE */
.time {
    font-size: 84px;
    font-weight: 300;
    color: white;
    letter-spacing: -2px;
    text-align: center;
}

.date {
    font-size: 20px;
    font-weight: 500;
    color: rgba(255,255,255,0.9);
    margin-top: 8px;
}

/* RONDS DE CODE */
#dots {
    display: flex;
    gap: 16px;
    margin: 40px 0;
}

.dot {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    border: 2px solid rgba(255,255,255,0.5);
    background: transparent;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}

.dot.filled {
    background: white;
    border-color: white;
    transform: scale(1.1);
}

/* MESSAGE */
#message {
    color: rgba(255,255,255,0.8);
    font-size: 14px;
    text-align: center;
    margin-bottom: 20px;
    height: 20px;
}

/* CLAVIER NUMÉRIQUE */
.keyboard {
    width: 100%;
    max-width: 390px;
    padding: 0 20px;
}

.keyboard-row {
    display: flex;
    justify-content: center;
    gap: 24px;
    margin-bottom: 20px;
}

.key {
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(255,255,255,0.2);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: all 0.15s;
    box-shadow: 0 4px 30px rgba(0,0,0,0.1);
}

.key:active {
    background: rgba(255,255,255,0.3);
    transform: scale(0.95);
}

.key-number {
    font-size: 36px;
    font-weight: 300;
    color: white;
    line-height: 1;
}

.key-letters {
    font-size: 10px;
    font-weight: 500;
    color: rgba(255,255,255,0.6);
    letter-spacing: 2px;
    margin-top: 2px;
}

/* BOUTON DELETE */
.key-delete {
    background: rgba(255,255,255,0.1);
}

.key-delete svg {
    width: 32px;
    height: 32px;
    fill: white;
}

/* ÉCRAN D'ACCUEIL */
#homescreen {
    width: 100vw;
    height: 100vh;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    background-size: cover;
    background-position: center;
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 100;
    overflow: hidden;
}

#homescreen.custom-bg {
    background-image: var(--home-bg);
}

#homescreen.show {
    display: block;
    animation: slideUp 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

@keyframes slideUp {
    from {
        transform: translateY(100%);
    }
    to {
        transform: translateY(0);
    }
}

/* SECRET MESSAGE */
#secret {
    position: absolute;
    bottom: 30px;
    right: 20px;
    color: rgba(255,255,255,0.3);
    font-size: 11px;
    text-align: right;
    line-height: 1.4;
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
    padding: 40px 20px;
    overflow-y: auto;
}

#setup h1 {
    font-size: 28px;
    color: white;
    margin-bottom: 40px;
    font-weight: 600;
}

.upload-box {
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    padding: 30px;
    margin-bottom: 20px;
    width: 100%;
    max-width: 340px;
    text-align: center;
}

.upload-box h3 {
    color: white;
    font-size: 18px;
    margin-bottom: 15px;
}

.upload-label {
    display: inline-block;
    background: white;
    color: #667eea;
    padding: 14px 32px;
    border-radius: 12px;
    font-weight: 600;
    cursor: pointer;
    transition: transform 0.2s;
}

.upload-label:active {
    transform: scale(0.95);
}

.upload-input {
    display: none;
}

.preview {
    margin-top: 15px;
    max-width: 100%;
    max-height: 150px;
    border-radius: 10px;
    display: none;
}

.preview.show {
    display: block;
}

#startBtn {
    background: white;
    color: #667eea;
    border: none;
    padding: 16px 48px;
    border-radius: 12px;
    font-size: 18px;
    font-weight: 600;
    cursor: pointer;
    margin-top: 30px;
    display: none;
    transition: transform 0.2s;
}

#startBtn.show {
    display: inline-block;
}

#startBtn:active {
    transform: scale(0.95);
}

.skip-btn {
    color: rgba(255,255,255,0.8);
    font-size: 16px;
    margin-top: 20px;
    cursor: pointer;
    text-decoration: underline;
}

/* ANIMATION SHAKE */
@keyframes shake {
    0%, 100% { transform: translateX(0); }
    25% { transform: translateX(-10px); }
    75% { transform: translateX(10px); }
}

.shake {
    animation: shake 0.4s;
}

/* SETTINGS BUTTON */
#settingsBtn {
    position: fixed;
    top: 20px;
    left: 20px;
    width: 44px;
    height: 44px;
    border-radius: 50%;
    background: rgba(0,0,0,0.3);
    backdrop-filter: blur(10px);
    border: 1px solid rgba(255,255,255,0.2);
    color: white;
    font-size: 20px;
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    cursor: pointer;
}

#settingsBtn.show {
    display: flex;
}

#menu {
    position: fixed;
    top: 75px;
    left: 20px;
    background: rgba(0,0,0,0.9);
    backdrop-filter: blur(20px);
    border-radius: 12px;
    padding: 10px;
    display: none;
    z-index: 1001;
    min-width: 200px;
}

#menu.show {
    display: block;
}

.menu-opt {
    padding: 12px;
    color: white;
    font-size: 14px;
    cursor: pointer;
    border-radius: 8px;
}

.menu-opt:hover {
    background: rgba(255,255,255,0.1);
}
</style>
</head>
<body>

<!-- SETUP -->
<div id="setup">
    <h1>📱 Personnalise ton iPhone</h1>
    
    <div class="upload-box">
        <h3>Fond d'écran verrouillé</h3>
        <label class="upload-label" for="lockBg">📸 Choisir</label>
        <input type="file" id="lockBg" class="upload-input" accept="image/*">
        <img id="prev1" class="preview">
    </div>
    
    <div class="upload-box">
        <h3>Fond d'écran accueil</h3>
        <label class="upload-label" for="homeBg">🏠 Choisir</label>
        <input type="file" id="homeBg" class="upload-input" accept="image/*">
        <img id="prev2" class="preview">
    </div>
    
    <button id="startBtn">🚀 Démarrer</button>
    <div class="skip-btn" onclick="skipSetup()">Passer (utiliser les fonds par défaut)</div>
</div>

<!-- LOCKSCREEN -->
<div id="lockscreen" style="display:none;">
    <div>
        <div class="time" id="time">12:00</div>
        <div class="date" id="date">Lundi 13 janvier</div>
    </div>
    
    <div style="text-align: center;">
        <div id="dots">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
        <div id="message"></div>
    </div>
    
    <div class="keyboard">
        <div class="keyboard-row">
            <div class="key" data-key="1">
                <div class="key-number">1</div>
                <div class="key-letters"></div>
            </div>
            <div class="key" data-key="2">
                <div class="key-number">2</div>
                <div class="key-letters">ABC</div>
            </div>
            <div class="key" data-key="3">
                <div class="key-number">3</div>
                <div class="key-letters">DEF</div>
            </div>
        </div>
        
        <div class="keyboard-row">
            <div class="key" data-key="4">
                <div class="key-number">4</div>
                <div class="key-letters">GHI</div>
            </div>
            <div class="key" data-key="5">
                <div class="key-number">5</div>
                <div class="key-letters">JKL</div>
            </div>
            <div class="key" data-key="6">
                <div class="key-number">6</div>
                <div class="key-letters">MNO</div>
            </div>
        </div>
        
        <div class="keyboard-row">
            <div class="key" data-key="7">
                <div class="key-number">7</div>
                <div class="key-letters">PQRS</div>
            </div>
            <div class="key" data-key="8">
                <div class="key-number">8</div>
                <div class="key-letters">TUV</div>
            </div>
            <div class="key" data-key="9">
                <div class="key-number">9</div>
                <div class="key-letters">WXYZ</div>
            </div>
        </div>
        
        <div class="keyboard-row">
            <div class="key" style="opacity:0; pointer-events:none;"></div>
            <div class="key" data-key="0">
                <div class="key-number">0</div>
            </div>
            <div class="key key-delete" onclick="deleteDigit()">
                <svg viewBox="0 0 24 24"><path d="M22 3H7c-.69 0-1.23.35-1.59.88L0 12l5.41 8.11c.36.53.9.89 1.59.89h15c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm-3 12.59L17.59 17 14 13.41 10.41 17 9 15.59 12.59 12 9 8.41 10.41 7 14 10.59 17.59 7 19 8.41 15.41 12 19 15.59z"/></svg>
            </div>
        </div>
    </div>
</div>

<!-- HOMESCREEN -->
<div id="homescreen">
    <div id="secret"></div>
</div>

<!-- SETTINGS -->
<button id="settingsBtn" onclick="toggleMenu()">⚙️</button>
<div id="menu">
    <div class="menu-opt" onclick="changeBg()">🖼️ Changer les fonds</div>
    <div class="menu-opt" onclick="resetApp()">🔄 Recommencer</div>
</div>

<script>
const CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

let input = "";
let firstCode = null;
let isUnlocked = false;
let lockBgData = null;
let homeBgData = null;

const setup = document.getElementById('setup');
const lockscreen = document.getElementById('lockscreen');
const homescreen = document.getElementById('homescreen');
const dots = document.querySelectorAll('.dot');
const message = document.getElementById('message');
const settingsBtn = document.getElementById('settingsBtn');

// Heure et date
function updateTime() {
    const now = new Date();
    const hours = String(now.getHours()).padStart(2, '0');
    const minutes = String(now.getMinutes()).padStart(2, '0');
    document.getElementById('time').textContent = `${hours}:${minutes}`;
    
    const days = ['Dimanche', 'Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi'];
    const months = ['janvier', 'février', 'mars', 'avril', 'mai', 'juin', 'juillet', 'août', 'septembre', 'octobre', 'novembre', 'décembre'];
    const day = days[now.getDay()];
    const date = now.getDate();
    const month = months[now.getMonth()];
    document.getElementById('date').textContent = `${day} ${date} ${month}`;
}

setInterval(updateTime, 1000);
updateTime();

// Upload images
document.getElementById('lockBg').onchange = e => {
    const file = e.target.files[0];
    if(file) {
        const reader = new FileReader();
        reader.onload = ev => {
            lockBgData = ev.target.result;
            document.getElementById('prev1').src = lockBgData;
            document.getElementById('prev1').classList.add('show');
            checkReady();
        };
        reader.readAsDataURL(file);
    }
};

document.getElementById('homeBg').onchange = e => {
    const file = e.target.files[0];
    if(file) {
        const reader = new FileReader();
        reader.onload = ev => {
            homeBgData = ev.target.result;
            document.getElementById('prev2').src = homeBgData;
            document.getElementById('prev2').classList.add('show');
            checkReady();
        };
        reader.readAsDataURL(file);
    }
};

function checkReady() {
    if(lockBgData && homeBgData) {
        document.getElementById('startBtn').classList.add('show');
    }
}

document.getElementById('startBtn').onclick = startApp;

function skipSetup() {
    startApp();
}

function startApp() {
    if(lockBgData) {
        lockscreen.style.setProperty('--bg-image', `url(${lockBgData})`);
        lockscreen.classList.add('custom-bg');
    }
    if(homeBgData) {
        homescreen.style.setProperty('--home-bg', `url(${homeBgData})`);
        homescreen.classList.add('custom-bg');
    }
    
    setup.style.display = 'none';
    lockscreen.style.display = 'flex';
    settingsBtn.classList.add('show');
}

// Keyboard
document.querySelectorAll('.key[data-key]').forEach(key => {
    key.onclick = () => {
        if(isUnlocked) return;
        const digit = key.dataset.key;
        handleDigit(digit);
    };
});

function handleDigit(d) {
    if(input.length >= 6 || isUnlocked) return;
    
    input += d;
    updateDots();
    
    if(navigator.vibrate) navigator.vibrate(10);
    
    console.log('Input:', input, 'FirstCode:', firstCode); // Debug
    
    if(input.length === 6) {
        setTimeout(() => {
            if(!firstCode) {
                console.log('Premier code mémorisé:', input);
                firstCode = input;
                message.textContent = "Entrer à nouveau le code";
                setTimeout(() => reset(), 500);
            } else if(CODES[input]) {
                console.log('Code valide! Déverrouillage...');
                message.textContent = "";
                unlock(CODES[input]);
            } else {
                console.log('Code incorrect');
                message.textContent = "Code incorrect";
                lockscreen.classList.add('shake');
                setTimeout(() => {
                    lockscreen.classList.remove('shake');
                    message.textContent = "Entrer à nouveau le code";
                    reset();
                }, 400);
            }
        }, 300);
    }
}

function deleteDigit() {
    if(input.length > 0) {
        input = input.slice(0, -1);
        updateDots();
        if(navigator.vibrate) navigator.vibrate(10);
    }
}

function updateDots() {
    dots.forEach((dot, i) => {
        if(i < input.length) {
            dot.classList.add('filled');
        } else {
            dot.classList.remove('filled');
        }
    });
}

function reset() {
    input = "";
    updateDots();
}

function unlock(celebrity) {
    console.log('Unlock function called for:', celebrity);
    isUnlocked = true;
    
    // Afficher le secret
    document.getElementById('secret').innerHTML = `${celebrity}<br>${firstCode}`;
    
    // Afficher l'écran d'accueil
    homescreen.style.display = 'block';
    homescreen.classList.add('show');
    
    // Cacher l'écran de verrouillage après l'animation
    setTimeout(() => {
        lockscreen.style.display = 'none';
    }, 400);
    
    // Vibration de succès
    if(navigator.vibrate) {
        navigator.vibrate([50, 100, 50]);
    }
    
    console.log('Homescreen should be visible now');
}

// Settings
function toggleMenu() {
    document.getElementById('menu').classList.toggle('show');
}

function changeBg() {
    isUnlocked = false;
    firstCode = null;
    input = "";
    message.textContent = "";
    homescreen.classList.remove('show');
    lockscreen.style.display = 'none';
    setup.style.display = 'flex';
    settingsBtn.classList.remove('show');
    document.getElementById('menu').classList.remove('show');
}

function resetApp() {
    location.reload();
}

// Close menu on click outside
document.addEventListener('click', (e) => {
    if(!e.target.closest('#settingsBtn') && !e.target.closest('#menu')) {
        document.getElementById('menu').classList.remove('show');
    }
});
</script>
</body>
</html>
