// Support tactile pour appui long
cont.addEventListener('touchstart', () => {
    if(unlock || calib) return;
    lp = setTimeout(() => {
        sBtn.classList.add('visible');
        if(navigator.vibrate) navigator.vibrate(50);
    }, 1500);
});
cont.addEventListener('touchend', () => clearTimeout(lp));
cont.addEventListener('touchmove', () => clearTimeout(lp));<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no, maximum-scale=1.0, minimum-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="">
<style>
* {
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    user-select: none;
    box-sizing: border-box;
}

html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    background: black;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

body.locked {
    overflow: hidden;
    position: fixed;
    height: 100%;
    width: 100%;
    touch-action: none;
}

#setup {
    width: 100%;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding: 40px 20px;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    overflow-y: auto;
    -webkit-overflow-scrolling: touch;
}

#setup h1 {
    font-size: 24px;
    margin-bottom: 10px;
    font-weight: 600;
}

#setup p {
    font-size: 14px;
    opacity: 0.9;
    margin-bottom: 30px;
    text-align: center;
    max-width: 320px;
}

.upload-section {
    background: rgba(255,255,255,0.15);
    backdrop-filter: blur(10px);
    border-radius: 16px;
    padding: 20px;
    margin-bottom: 15px;
    width: 100%;
    max-width: 340px;
}

.upload-section h3 {
    font-size: 16px;
    margin: 0 0 12px 0;
}

.upload-label {
    display: block;
    background: white;
    color: #667eea;
    padding: 12px 24px;
    border-radius: 12px;
    cursor: pointer;
    text-align: center;
    font-weight: 600;
    font-size: 14px;
}

.upload-input {
    display: none;
}

.preview {
    margin-top: 12px;
    border-radius: 8px;
    max-width: 100%;
    max-height: 200px;
    display: none;
}

.preview.visible {
    display: block;
}

#startBtn {
    background: white;
    color: #667eea;
    border: none;
    padding: 16px 48px;
    border-radius: 12px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    margin-top: 20px;
    margin-bottom: 40px;
    display: none;
}

#startBtn.visible {
    display: block;
}

#container {
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    display: none;
    overflow: hidden;
}

#container.visible {
    display: block;
}

#phone {
    width: 100%;
    height: 100%;
    position: absolute;
    background-size: 100% 100%;
    background-position: center;
    touch-action: none;
    user-select: none;
}

.touch {
    position: absolute;
    z-index: 10;
    cursor: pointer;
}

body.debug .touch {
    background: rgba(0,255,0,0.3);
    border: 2px solid green;
}

body.calib .touch {
    background: rgba(255,0,0,0.5);
    border: 3px solid red;
    cursor: move;
}

#unlocked {
    display: none;
    position: absolute;
    inset: 0;
    background-size: 100% 100%;
    background-position: center;
    z-index: 100;
}

#secret {
    position: absolute;
    bottom: 20px;
    right: 15px;
    font-size: 10px;
    opacity: 0.25;
    color: white;
    text-align: right;
}

#settingsBtn {
    position: fixed;
    top: 20px;
    left: 20px;
    width: 50px;
    height: 50px;
    background: rgba(0,0,0,0.8);
    color: white;
    border: 2px solid white;
    border-radius: 50%;
    font-size: 24px;
    z-index: 10000;
    cursor: pointer;
    display: none;
    align-items: center;
    justify-content: center;
}

#settingsBtn.visible {
    display: flex;
}

#menu {
    display: none;
    position: fixed;
    top: 80px;
    left: 20px;
    background: rgba(0,0,0,0.95);
    border: 1px solid white;
    border-radius: 12px;
    padding: 10px;
    z-index: 10001;
    color: white;
    font-size: 13px;
    min-width: 180px;
}

#menu.visible {
    display: block;
}

.opt {
    padding: 10px;
    cursor: pointer;
    border-bottom: 1px solid rgba(255,255,255,0.2);
}

.opt:last-child {
    border-bottom: none;
}

.opt:hover {
    background: rgba(255,255,255,0.1);
}

/* Bouton de validation en mode calibration */
#saveBtn {
    display: none;
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: #4CAF50;
    color: white;
    border: none;
    padding: 15px 40px;
    border-radius: 12px;
    font-size: 16px;
    font-weight: 600;
    z-index: 10003;
    cursor: pointer;
    box-shadow: 0 4px 15px rgba(0,0,0,0.5);
}

body.calib #saveBtn {
    display: block;
}

#info {
    display: none;
    position: fixed;
    bottom: 80px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0,0,0,0.95);
    color: white;
    padding: 15px 25px;
    border-radius: 12px;
    font-size: 12px;
    z-index: 10002;
    text-align: center;
    max-width: 80%;
}

body.calib #info {
    display: block;
}

@keyframes shake {
    0%, 100% { transform: translate(0, 0); }
    25% { transform: translate(-10px, 0); }
    75% { transform: translate(10px, 0); }
}

.shake {
    animation: shake 0.4s;
}
</style>
</head>
<body>

<div id="setup">
    <h1>📱 iPhone Lock Screen</h1>
    <p>Upload tes captures d'écran</p>
    
    <div class="upload-section">
        <h3>1️⃣ Écran verrouillé</h3>
        <label for="lock" class="upload-label">📸 Choisir</label>
        <input type="file" id="lock" class="upload-input" accept="image/*">
        <img id="prev1" class="preview">
    </div>
    
    <div class="upload-section">
        <h3>2️⃣ Écran d'accueil</h3>
        <label for="home" class="upload-label">🏠 Choisir</label>
        <input type="file" id="home" class="upload-input" accept="image/*">
        <img id="prev2" class="preview">
    </div>
    
    <button id="startBtn">🚀 Démarrer</button>
</div>

<div id="container">
    <div id="phone">
        <div class="touch" data-d="1" style="left:13%; top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="2" style="left:41%; top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="3" style="left:69%; top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="4" style="left:13%; top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="5" style="left:41%; top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="6" style="left:69%; top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="7" style="left:13%; top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="8" style="left:41%; top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="9" style="left:69%; top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-d="0" style="left:41%; top:91.5%; width:18%; height:8.5%;"></div>
        <div id="unlocked"><div id="secret"></div></div>
    </div>
</div>

<button id="settingsBtn">⚙️</button>
<div id="menu">
    <div class="opt" onclick="toggleCalib()">🎯 Déplacer zones</div>
    <div class="opt" onclick="toggleDebug()">👁️ Voir zones</div>
    <div class="opt" onclick="back()">🖼️ Changer images</div>
</div>
<div id="info">Glisse les zones rouges<br>Clique "Valider" quand c'est OK</div>
<button id="saveBtn" onclick="saveCalib()">✓ Valider les positions</button>

<script>
const CODES = {"123456":"Brad Pitt","654321":"Emma Watson","111111":"Leonardo DiCaprio"};
let inp = "", first = null, unlock = false, calib = false, debug = false;
let lockData, homeData, drag = null;

const phone = document.getElementById('phone');
const setup = document.getElementById('setup');
const cont = document.getElementById('container');
const btn = document.getElementById('startBtn');
const sBtn = document.getElementById('settingsBtn');

document.getElementById('lock').onchange = e => {
    const f = e.target.files[0];
    if(f) {
        const r = new FileReader();
        r.onload = ev => {
            lockData = ev.target.result;
            document.getElementById('prev1').src = lockData;
            document.getElementById('prev1').classList.add('visible');
            check();
        };
        r.readAsDataURL(f);
    }
};

document.getElementById('home').onchange = e => {
    const f = e.target.files[0];
    if(f) {
        const r = new FileReader();
        r.onload = ev => {
            homeData = ev.target.result;
            document.getElementById('prev2').src = homeData;
            document.getElementById('prev2').classList.add('visible');
            check();
        };
        r.readAsDataURL(f);
    }
};

function check() {
    if(lockData && homeData) btn.classList.add('visible');
}

btn.onclick = () => {
    phone.style.backgroundImage = `url(${lockData})`;
    document.getElementById('unlocked').style.backgroundImage = `url(${homeData})`;
    setup.style.display = 'none';
    cont.classList.add('visible');
    document.body.classList.add('locked');
};

let lp = null;
cont.addEventListener('mousedown', () => {
    if(unlock || calib) return;
    lp = setTimeout(() => {
        sBtn.classList.add('visible');
        if(navigator.vibrate) navigator.vibrate(50);
    }, 1500);
});
cont.addEventListener('mouseup', () => clearTimeout(lp));
cont.addEventListener('mousemove', () => clearTimeout(lp));

sBtn.onclick = e => {
    e.stopPropagation();
    document.getElementById('menu').classList.toggle('visible');
};

function toggleCalib() {
    calib = !calib;
    if(calib) {
        document.body.classList.add('calib');
        document.body.classList.remove('debug');
        debug = false;
    } else {
        document.body.classList.remove('calib');
    }
    document.getElementById('menu').classList.remove('visible');
}

function saveCalib() {
    // Désactiver le mode calibration - les zones deviennent tactiles
    calib = false;
    document.body.classList.remove('calib');
    alert('✓ Zones sauvegardées ! Elles sont maintenant actives.');
}

function toggleDebug() {
    debug = !debug;
    document.body.classList.toggle('debug');
    document.body.classList.remove('calib');
    calib = false;
    document.getElementById('menu').classList.remove('visible');
}

function back() {
    sBtn.classList.remove('visible');
    document.body.classList.remove('locked','calib','debug');
    cont.classList.remove('visible');
    setup.style.display = 'flex';
    document.getElementById('menu').classList.remove('visible');
}

function reset() { inp = ""; }

function err() {
    phone.classList.add('shake');
    setTimeout(() => { phone.classList.remove('shake'); reset(); }, 400);
}

function unlockFn(c) {
    unlock = true;
    document.getElementById('unlocked').style.display = 'block';
    document.getElementById('secret').innerHTML = `${c}<br>${first}`;
    if(navigator.vibrate) navigator.vibrate([50,100,50]);
}

function handle(d) {
    if(unlock || calib) return;
    
    inp += d;
    console.log('Input:', inp, 'Length:', inp.length); // Debug
    
    if(inp.length === 6) {
        setTimeout(() => {
            if(!first) { 
                console.log('Premier code mémorisé:', inp);
                first = inp; 
                reset(); 
            }
            else if(CODES[inp]) {
                console.log('Code valide! Déverrouillage...');
                unlockFn(CODES[inp]);
            }
            else {
                console.log('Code invalide! Shake...');
                err();
            }
        }, 300);
    }
}

document.querySelectorAll('.touch').forEach(z => {
    let st = 0;
    let handled = false;
    
    // === SOURIS (ordinateur) ===
    z.addEventListener('mousedown', e => {
        st = Date.now();
        handled = false;
        
        if(!calib) return;
        
        drag = z;
        const r = z.getBoundingClientRect();
        z._ox = e.clientX - r.left;
        z._oy = e.clientY - r.top;
        e.preventDefault();
        e.stopPropagation();
    });
    
    z.addEventListener('mouseup', e => {
        const dur = Date.now() - st;
        
        if(drag === z) { 
            drag = null;
            e.preventDefault();
            e.stopPropagation();
            return;
        }
        
        if(!calib && !debug && dur < 500 && !handled) {
            handled = true;
            handle(z.dataset.d);
            e.preventDefault();
            e.stopPropagation();
        }
    });
    
    // === TACTILE (iPhone) ===
    z.addEventListener('touchstart', e => {
        st = Date.now();
        handled = false;
        
        if(!calib) return;
        
        const t = e.touches[0];
        drag = z;
        const r = z.getBoundingClientRect();
        z._ox = t.clientX - r.left;
        z._oy = t.clientY - r.top;
        e.preventDefault();
    });
    
    z.addEventListener('touchend', e => {
        const dur = Date.now() - st;
        
        if(drag === z) { 
            drag = null;
            e.preventDefault();
            return;
        }
        
        if(!calib && !debug && dur < 500 && !handled) {
            handled = true;
            handle(z.dataset.d);
            e.preventDefault();
        }
    });
});

// Drag global pour souris
document.onmousemove = e => {
    if(!drag || !calib) return;
    const pr = phone.getBoundingClientRect();
    let nl = ((e.clientX - pr.left - drag._ox) / pr.width) * 100;
    let nt = ((e.clientY - pr.top - drag._oy) / pr.height) * 100;
    nl = Math.max(0, Math.min(nl, 100 - parseFloat(drag.style.width)));
    nt = Math.max(0, Math.min(nt, 100 - parseFloat(drag.style.height)));
    drag.style.left = nl + '%';
    drag.style.top = nt + '%';
    e.preventDefault();
};

document.onmouseup = () => { if(drag) drag = null; };

// Drag global pour tactile
document.addEventListener('touchmove', e => {
    if(!drag || !calib) return;
    const t = e.touches[0];
    const pr = phone.getBoundingClientRect();
    let nl = ((t.clientX - pr.left - drag._ox) / pr.width) * 100;
    let nt = ((t.clientY - pr.top - drag._oy) / pr.height) * 100;
    nl = Math.max(0, Math.min(nl, 100 - parseFloat(drag.style.width)));
    nt = Math.max(0, Math.min(nt, 100 - parseFloat(drag.style.height)));
    drag.style.left = nl + '%';
    drag.style.top = nt + '%';
    e.preventDefault();
}, {passive: false});

document.addEventListener('touchend', () => { if(drag) drag = null; });

// Empêcher le zoom et autres comportements indésirables
document.addEventListener('gesturestart', e => e.preventDefault());
document.addEventListener('gesturechange', e => e.preventDefault());
document.addEventListener('gestureend', e => e.preventDefault());

// Empêcher le scroll sur le container
cont.addEventListener('touchmove', e => {
    if(!calib) e.preventDefault();
}, {passive: false});
</script>
</body>
</html>
