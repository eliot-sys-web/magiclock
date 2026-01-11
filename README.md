<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone Lock</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<style>
html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    touch-action: none;
    background: black;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

/* ÉCRAN DE CONFIGURATION */
#setup {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 20px;
    box-sizing: border-box;
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
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
    font-weight: 500;
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
    transition: transform 0.2s;
}

.upload-label:active {
    transform: scale(0.95);
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
    border: 2px solid rgba(255,255,255,0.3);
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
    display: none;
    transition: transform 0.2s;
}

#startBtn:active {
    transform: scale(0.95);
}

#startBtn.visible {
    display: block;
}

/* CONTENEUR PRINCIPAL */
#container {
    width: 100%;
    height: 100%;
    position: relative;
    display: none;
    align-items: center;
    justify-content: center;
}

#container.visible {
    display: flex;
}

#phone {
    width: 100vw;
    height: 100vh;
    position: relative;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
}

/* FEEDBACK VISUEL DU CODE */
#codeDisplay {
    position: absolute;
    top: 400px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    gap: 12px;
    z-index: 10;
}

.dot {
    width: 14px;
    height: 14px;
    border-radius: 50%;
    border: 2px solid rgba(255,255,255,0.6);
    background: transparent;
    transition: all 0.2s;
}

.dot.filled {
    background: white;
    border-color: white;
}

/* ÉCRAN DÉVERROUILLÉ */
#unlocked {
    display: none;
    position: absolute;
    inset: 0;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
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
    line-height: 1.3;
}

/* ZONES TACTILES */
.touch {
    position: absolute;
    border-radius: 50%;
    z-index: 5;
}

/* MODE CALIBRATION */
body.calibration .touch {
    background: rgba(255,0,0,0.3);
    border: 2px solid red;
}

body.calibration #calibBtn {
    display: block;
}

#calibBtn {
    display: none;
    position: fixed;
    top: 10px;
    right: 10px;
    padding: 10px 20px;
    background: rgba(0,0,0,0.8);
    color: white;
    border: 1px solid white;
    border-radius: 8px;
    font-size: 12px;
    z-index: 1000;
    cursor: pointer;
}

@keyframes shake {
    0%, 100% { transform: translateX(-50%); }
    25% { transform: translateX(-45%); }
    75% { transform: translateX(-55%); }
}

.shake #codeDisplay {
    animation: shake 0.4s;
}
</style>
</head>
<body>

<!-- ÉCRAN DE CONFIGURATION -->
<div id="setup">
    <h1>📱 iPhone Lock Screen</h1>
    <p>Upload tes captures d'écran pour créer ton écran de verrouillage personnalisé</p>
    
    <div class="upload-section">
        <h3>1️⃣ Écran verrouillé</h3>
        <label for="lockscreen" class="upload-label">
            📸 Choisir l'image
        </label>
        <input type="file" id="lockscreen" class="upload-input" accept="image/*">
        <img id="lockPreview" class="preview">
    </div>
    
    <div class="upload-section">
        <h3>2️⃣ Écran d'accueil</h3>
        <label for="homescreen" class="upload-label">
            🏠 Choisir l'image
        </label>
        <input type="file" id="homescreen" class="upload-input" accept="image/*">
        <img id="homePreview" class="preview">
    </div>
    
    <button id="startBtn">🚀 Démarrer</button>
</div>

<!-- ÉCRAN PRINCIPAL -->
<div id="container">
    <div id="phone">
        <div id="codeDisplay">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>

        <div class="touch" data-digit="1" style="left:55px;  top:530px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="2" style="left:160px; top:530px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="3" style="left:265px; top:530px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="4" style="left:55px;  top:615px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="5" style="left:160px; top:615px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="6" style="left:265px; top:615px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="7" style="left:55px;  top:700px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="8" style="left:160px; top:700px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="9" style="left:265px; top:700px; width:70px; height:70px;"></div>
        <div class="touch" data-digit="0" style="left:160px; top:785px; width:70px; height:70px;"></div>

        <div id="unlocked">
            <div id="secret"></div>
        </div>
    </div>
</div>

<button id="calibBtn" onclick="toggleCalibration()">Terminer calibration</button>

<script>
// 🔐 CODES VALIDES
const VALID_CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

let input = "";
let firstCode = null;
let isUnlocked = false;
let lockscreenData = null;
let homescreenData = null;

const dots = document.querySelectorAll('.dot');
const phone = document.getElementById('phone');
const setup = document.getElementById('setup');
const container = document.getElementById('container');
const startBtn = document.getElementById('startBtn');

// GESTION DES UPLOADS
document.getElementById('lockscreen').addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
            lockscreenData = event.target.result;
            document.getElementById('lockPreview').src = lockscreenData;
            document.getElementById('lockPreview').classList.add('visible');
            checkReady();
        };
        reader.readAsDataURL(file);
    }
});

document.getElementById('homescreen').addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
            homescreenData = event.target.result;
            document.getElementById('homePreview').src = homescreenData;
            document.getElementById('homePreview').classList.add('visible');
            checkReady();
        };
        reader.readAsDataURL(file);
    }
});

function checkReady() {
    if (lockscreenData && homescreenData) {
        startBtn.classList.add('visible');
    }
}

startBtn.addEventListener('click', function() {
    phone.style.backgroundImage = `url(${lockscreenData})`;
    document.getElementById('unlocked').style.backgroundImage = `url(${homescreenData})`;
    setup.style.display = 'none';
    container.classList.add('visible');
});

// MODE CALIBRATION
let calibrationMode = false;
let longPressTimer;

phone.addEventListener('touchstart', (e) => {
    if (isUnlocked) return;
    longPressTimer = setTimeout(() => {
        calibrationMode = true;
        document.body.classList.add('calibration');
        navigator.vibrate && navigator.vibrate(50);
    }, 2000);
});

phone.addEventListener('touchend', () => {
    clearTimeout(longPressTimer);
});

phone.addEventListener('touchmove', () => {
    clearTimeout(longPressTimer);
});

function toggleCalibration() {
    calibrationMode = false;
    document.body.classList.remove('calibration');
}

// MISE À JOUR VISUELLE
function updateDots() {
    dots.forEach((dot, i) => {
        if (i < input.length) {
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

function showError() {
    phone.classList.add('shake');
    setTimeout(() => {
        phone.classList.remove('shake');
        reset();
    }, 400);
}

function unlock(celebrity) {
    isUnlocked = true;
    document.getElementById("unlocked").style.display = "block";
    document.getElementById("secret").innerHTML = 
        `${celebrity}<br>${firstCode}`;
    
    if (navigator.vibrate) {
        navigator.vibrate([50, 100, 50]);
    }
}

function handleDigit(digit) {
    if (isUnlocked || calibrationMode) return;
    if (input.length >= 6) return;
    
    if (navigator.vibrate) {
        navigator.vibrate(10);
    }
    
    input += digit;
    updateDots();
    
    if (input.length === 6) {
        setTimeout(() => {
            if (!firstCode) {
                firstCode = input;
                reset();
            } else if (VALID_CODES[input]) {
                unlock(VALID_CODES[input]);
            } else {
                showError();
            }
        }, 300);
    }
}

// ZONES TACTILES
document.querySelectorAll(".touch").forEach(zone => {
    zone.addEventListener("click", (e) => {
        e.preventDefault();
        handleDigit(zone.dataset.digit);
    });
    
    zone.addEventListener("touchend", (e) => {
        e.preventDefault();
        handleDigit(zone.dataset.digit);
    });
});

document.addEventListener('gesturestart', (e) => {
    e.preventDefault();
});
</script>
</body>
</html>
