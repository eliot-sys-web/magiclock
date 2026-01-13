<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone Lock Screen</title>

<!-- Viewport pour mobile -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">

<!-- Mode plein écran iOS -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="Lockscreen">

<!-- Icône iOS écran d'accueil (180x180 px recommandé) -->
<link rel="apple-touch-icon" href="Lock180.png">

<!-- Manifest PWA pour Android/Chrome -->
<link rel="manifest" href="manifest.json">
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
    user-select: none;
}

body {
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
    overflow: hidden;
    background: #000;
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
}

/* LOCKSCREEN - Screenshot en fond */
#lockscreen {
    width: 100vw;
    height: 100vh;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0;
    left: 0;
    background-color: #000;
}

#lockscreen.custom {
    background-image: var(--bg-lock);
}

/* Overlay pour les ronds dynamiques */
#dots-overlay {
    position: absolute;
    top: 24.5%; /* Ajustable selon ton écran */
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 17px;
    z-index: 10;
}

.dot {
    width: 18px;
    height: 18px;
    border-radius: 50%;
    border: 2px solid rgba(255,255,255,0);
    background: transparent;
    transition: all 0.15s ease;
}

.dot.filled {
    background: white;
    border-color: white;
}

/* Zones tactiles invisibles sur le clavier */
.touch-zone {
    position: absolute;
    border-radius: 50%;
    z-index: 5;
    cursor: pointer;
}

/* Pour debug : afficher les zones */
body.debug .touch-zone {
    background: rgba(255,0,0,0.3);
    border: 2px solid red;
}

/* HOMESCREEN */
#homescreen {
    width: 100vw;
    height: 100vh;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 200;
    background-color: #000;
}

#homescreen.custom {
    background-image: var(--bg-home);
}

#homescreen.show {
    display: block;
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
    position: fixed;
    top: 0;
    left: 0;
}

#setup h1 {
    font-size: 26px;
    color: white;
    margin-bottom: 30px;
    font-weight: 600;
    text-align: center;
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

.skip { 
    color: rgba(255,255,255,0.8); 
    margin-top: 16px; 
    cursor: pointer; 
    font-size: 15px;
    text-decoration: underline;
}

.info {
    background: rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 16px;
    margin-top: 20px;
    max-width: 340px;
    color: white;
    font-size: 13px;
    line-height: 1.5;
}

/* SETTINGS */
#settingsBtn {
    position: fixed;
    top: 60px;
    left: 20px;
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: rgba(0,0,0,0.4);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(255,255,255,0.2);
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
    0%, 100% { transform: translateX(-50%); }
    25% { transform: translateX(calc(-50% - 10px)); }
    75% { transform: translateX(calc(-50% + 10px)); }
}

#dots-overlay.shake {
    animation: shake 0.35s;
}
</style>
</head>
<body>

<!-- SETUP -->
<div id="setup">
    <h1>📱 iPhone Lock Screen<br>Overlay Method</h1>
    
    <div class="upload-box">
        <h3>Screenshot écran verrouillé</h3>
        <label class="upload-label" for="lockBg">Choisir</label>
        <input type="file" id="lockBg" class="upload-input" accept="image/*">
        <img id="prev1" class="preview">
    </div>
    
    <div class="upload-box">
        <h3>Screenshot écran d'accueil</h3>
        <label class="upload-label" for="homeBg">Choisir</label>
        <input type="file" id="homeBg" class="upload-input" accept="image/*">
        <img id="prev2" class="preview">
    </div>
    
    <button id="startBtn">Démarrer</button>
    
    <div class="info">
        💡 <strong>Astuce :</strong> Les ronds se superposeront à ton screenshot. Utilise les réglages pour ajuster leur position si besoin.
    </div>
</div>

<!-- LOCKSCREEN -->
<div id="lockscreen" style="display:none;">
    <!-- Ronds dynamiques superposés -->
    <div id="dots-overlay">
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
        <div class="dot"></div>
    </div>
    
    <!-- Zones tactiles invisibles (positionnées sur les touches du screenshot) -->
    <!-- Basé sur ton screenshot iPhone 13 -->
    <div class="touch-zone" data-k="1" style="left:14%; top:36%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="2" style="left:39%; top:36%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="3" style="left:64%; top:36%; width:22%; height:9%;"></div>
    
    <div class="touch-zone" data-k="4" style="left:14%; top:47%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="5" style="left:39%; top:47%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="6" style="left:64%; top:47%; width:22%; height:9%;"></div>
    
    <div class="touch-zone" data-k="7" style="left:14%; top:58%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="8" style="left:39%; top:58%; width:22%; height:9%;"></div>
    <div class="touch-zone" data-k="9" style="left:64%; top:58%; width:22%; height:9%;"></div>
    
    <div class="touch-zone" data-k="0" style="left:39%; top:69%; width:22%; height:9%;"></div>
</div>

<!-- HOMESCREEN -->
<div id="homescreen">
    <div id="secret"></div>
</div>

<button id="settingsBtn" onclick="menu()">⚙️</button>
<div id="menu">
    <div class="menu-opt" onclick="toggleDebug()">👁️ Voir les zones</div>
    <div class="menu-opt" onclick="adjustDots()">⚪ Ajuster les ronds</div>
    <div class="menu-opt" onclick="change()">🖼️ Changer screenshots</div>
    <div class="menu-opt" onclick="location.reload()">🔄 Recommencer</div>
</div>

<script>
const CODES = {"123456":"Brad Pitt","654321":"Emma Watson","111111":"Leonardo DiCaprio"};
let inp = "", first = null, unlock = false;
let lockBg = null, homeBg = null;

const setup = document.getElementById('setup');
const lock = document.getElementById('lockscreen');
const home = document.getElementById('homescreen');
const dotsOverlay = document.getElementById('dots-overlay');
const dots = document.querySelectorAll('.dot');
const btn = document.getElementById('settingsBtn');

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

// Zones tactiles
document.querySelectorAll('.touch-zone').forEach(z => {
    z.onclick = () => { if(!unlock) handle(z.dataset.k); };
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
                setTimeout(() => reset(), 600);
            } else if(CODES[inp]) {
                unlockScreen(CODES[inp]);
            } else {
                dotsOverlay.classList.add('shake');
                setTimeout(() => {
                    dotsOverlay.classList.remove('shake');
                    reset();
                }, 400);
            }
        }, 300);
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
    lock.style.display = 'none';
    if(navigator.vibrate) navigator.vibrate([50,100,50]);
}

// Settings
function menu() {
    document.getElementById('menu').classList.toggle('show');
}

function toggleDebug() {
    document.body.classList.toggle('debug');
    document.getElementById('menu').classList.remove('show');
}

let adjustMode = false;
let draggedDot = null;

function adjustDots() {
    adjustMode = !adjustMode;
    if(adjustMode) {
        dotsOverlay.style.background = 'rgba(255,255,0,0.2)';
        dotsOverlay.style.padding = '10px';
        dotsOverlay.style.border = '2px dashed orange';
        dotsOverlay.style.cursor = 'move';
        
        // Rendre draggable
        dotsOverlay.onmousedown = startDrag;
        dotsOverlay.ontouchstart = startDrag;
    } else {
        dotsOverlay.style.background = '';
        dotsOverlay.style.padding = '';
        dotsOverlay.style.border = '';
        dotsOverlay.style.cursor = '';
        dotsOverlay.onmousedown = null;
        dotsOverlay.ontouchstart = null;
    }
    document.getElementById('menu').classList.remove('show');
}

function startDrag(e) {
    if(!adjustMode) return;
    draggedDot = dotsOverlay;
    const isTouch = e.type === 'touchstart';
    const clientX = isTouch ? e.touches[0].clientX : e.clientX;
    const clientY = isTouch ? e.touches[0].clientY : e.clientY;
    
    const rect = dotsOverlay.getBoundingClientRect();
    dotsOverlay._offsetX = clientX - rect.left;
    dotsOverlay._offsetY = clientY - rect.top;
    
    e.preventDefault();
}

document.addEventListener('mousemove', doDrag);
document.addEventListener('touchmove', doDrag);

function doDrag(e) {
    if(!draggedDot) return;
    const isTouch = e.type === 'touchmove';
    const clientX = isTouch ? e.touches[0].clientX : e.clientX;
    const clientY = isTouch ? e.touches[0].clientY : e.clientY;
    
    const lockRect = lock.getBoundingClientRect();
    const newLeft = ((clientX - lockRect.left - dotsOverlay._offsetX) / lockRect.width) * 100;
    const newTop = ((clientY - lockRect.top - dotsOverlay._offsetY) / lockRect.height) * 100;
    
    dotsOverlay.style.left = newLeft + '%';
    dotsOverlay.style.top = newTop + '%';
    dotsOverlay.style.transform = 'none';
    
    e.preventDefault();
}

document.addEventListener('mouseup', () => { draggedDot = null; });
document.addEventListener('touchend', () => { draggedDot = null; });

function change() {
    unlock = false;
    first = null;
    inp = "";
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
