<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone</title>
<!-- PWA Configuration -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no, maximum-scale=1.0, minimum-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="iPhone">
<meta name="mobile-web-app-capable" content="yes">
<link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiaVBob25lIiwic2hvcnRfbmFtZSI6ImlQaG9uZSIsImRpc3BsYXkiOiJmdWxsc2NyZWVuIiwib3JpZW50YXRpb24iOiJwb3J0cmFpdCJ9">
<style>
* {
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    user-select: none;
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
    touch-action: none;
    position: fixed;
    height: 100%;
    overscroll-behavior: none;
}

/* ÉCRAN DE CONFIGURATION */
#setup {
    width: 100%;
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    padding: 40px 20px;
    box-sizing: border-box;
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
    line-height: 1.5;
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
    margin-bottom: 40px;
    display: none;
    transition: transform 0.2s;
}

#startBtn:active {
    transform: scale(0.95);
}

#startBtn.visible {
    display: block;
}

.info-box {
    background: rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 15px;
    margin-top: 20px;
    max-width: 340px;
    font-size: 12px;
    line-height: 1.5;
}

/* CONTENEUR PRINCIPAL */
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
    top: 0;
    left: 0;
    /* ALIGNEMENT PIXEL-PERFECT */
    background-size: 100% 100%;
    background-position: center center;
    background-repeat: no-repeat;
}

/* FEEDBACK VISUEL DU CODE - CACHÉ PAR DÉFAUT */
#codeDisplay {
    position: absolute;
    top: 48%;
    left: 50%;
    transform: translateX(-50%);
    display: none !important;
    gap: 12px;
    z-index: 10;
}

/* Option pour afficher les points si besoin */
body.show-dots #codeDisplay {
    display: flex !important;
}

body.show-dots #codeDisplay.active {
    display: flex !important;
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
    background-size: 100% 100%;
    background-position: center center;
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

body.calibration #codeDisplay {
    display: none;
}

#calibBtn {
    display: none;
    position: fixed;
    top: env(safe-area-inset-top, 10px);
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
    
    <div class="info-box">
        💡 <strong>Astuce :</strong> Une fois lancé, clique sur ⚙️ en haut à gauche pour accéder aux options (calibration des zones tactiles, affichage des points, etc.)
    </div>
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

        <!-- Zones tactiles ajustées pour iPhone 13 -->
        <div class="touch" data-digit="1" style="left:13%;  top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="2" style="left:41%;  top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="3" style="left:69%;  top:63%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="4" style="left:13%;  top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="5" style="left:41%;  top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="6" style="left:69%;  top:72.5%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="7" style="left:13%;  top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="8" style="left:41%;  top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="9" style="left:69%;  top:82%; width:18%; height:8.5%;"></div>
        <div class="touch" data-digit="0" style="left:41%;  top:91.5%; width:18%; height:8.5%;"></div>

        <div id="unlocked">
            <div id="secret"></div>
        </div>
    </div>
    
    <!-- Bouton settings -->
    <button id="settingsBtn">⚙️</button>
    <div id="settingsMenu">
        <div class="menu-option" onclick="toggleCalibrationMode()">🎯 Mode calibration</div>
        <div class="menu-option" onclick="openDotsControl()">🎚️ Ajuster les points</div>
        <div class="menu-option" onclick="resetApp()">🔄 Recommencer</div>
    </div>
    
    <!-- Panel de contrôle des points -->
    <div id="dotsControl">
        <h3 style="margin: 0 0 15px 0; font-size: 14px;">🎯 Ajuster les points</h3>
        
        <div class="control-group">
            <label>Position verticale</label>
            <input type="range" id="dotsTop" min="30" max="60" value="48" step="0.5">
            <div class="control-values">
                <span>Haut</span>
                <span id="dotsTopValue">48%</span>
                <span>Bas</span>
            </div>
        </div>
        
        <div class="control-group">
            <label>Position horizontale</label>
            <input type="range" id="dotsLeft" min="40" max="60" value="50" step="0.5">
            <div class="control-values">
                <span>Gauche</span>
                <span id="dotsLeftValue">50%</span>
                <span>Droite</span>
            </div>
        </div>
        
        <div class="control-group">
            <label>Espacement entre les points</label>
            <input type="range" id="dotsGap" min="6" max="20" value="12" step="1">
            <div class="control-values">
                <span>Serré</span>
                <span id="dotsGapValue">12px</span>
                <span>Large</span>
            </div>
        </div>
        
        <div class="control-group">
            <label>Taille des points</label>
            <input type="range" id="dotsSize" min="8" max="20" value="14" step="1">
            <div class="control-values">
                <span>Petit</span>
                <span id="dotsSizeValue">14px</span>
                <span>Grand</span>
            </div>
        </div>
        
        <div class="control-buttons">
            <button class="btn-close" onclick="closeDotsControl()">Fermer</button>
            <button class="btn-apply" onclick="applyDotsSettings()">✓ Valider</button>
        </div>
    </div>
</div>

<button id="calibBtn" onclick="toggleCalibration()">✓ Terminer calibration</button>

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
    document.body.classList.add('locked');
});

// MODE CALIBRATION - Désactivé par défaut
let calibrationMode = false;
let showDotsMode = false;

// Suppression de l'appui long automatique
// Plus de déclenchement accidentel !

function toggleCalibrationMode() {
    calibrationMode = !calibrationMode;
    if (calibrationMode) {
        document.body.classList.add('calibration');
        document.getElementById('settingsMenu').classList.remove('visible');
    } else {
        document.body.classList.remove('calibration');
    }
}

function openDotsControl() {
    document.body.classList.add('show-dots');
    document.getElementById('dotsControl').classList.add('visible');
    document.getElementById('settingsMenu').classList.remove('visible');
    showDotsMode = true;
}

function closeDotsControl() {
    document.getElementById('dotsControl').classList.remove('visible');
}

function applyDotsSettings() {
    document.body.classList.remove('show-dots');
    document.getElementById('dotsControl').classList.remove('visible');
    showDotsMode = false;
}

function resetApp() {
    location.reload();
}

function toggleCalibration() {
    calibrationMode = false;
    document.body.classList.remove('calibration');
}

// Menu settings
document.getElementById('settingsBtn').addEventListener('click', (e) => {
    e.stopPropagation();
    const menu = document.getElementById('settingsMenu');
    menu.classList.toggle('visible');
});

// Fermer le menu si on clique ailleurs
document.addEventListener('click', () => {
    document.getElementById('settingsMenu').classList.remove('visible');
});

// Contrôle des sliders en temps réel
const dotsDisplay = document.getElementById('codeDisplay');

document.getElementById('dotsTop').addEventListener('input', function() {
    const value = this.value;
    dotsDisplay.style.top = value + '%';
    document.getElementById('dotsTopValue').textContent = value + '%';
});

document.getElementById('dotsLeft').addEventListener('input', function() {
    const value = this.value;
    dotsDisplay.style.left = value + '%';
    document.getElementById('dotsLeftValue').textContent = value + '%';
});

document.getElementById('dotsGap').addEventListener('input', function() {
    const value = this.value;
    dotsDisplay.style.gap = value + 'px';
    document.getElementById('dotsGapValue').textContent = value + 'px';
});

document.getElementById('dotsSize').addEventListener('input', function() {
    const value = this.value;
    const dots = document.querySelectorAll('.dot');
    dots.forEach(dot => {
        dot.style.width = value + 'px';
        dot.style.height = value + 'px';
    });
    document.getElementById('dotsSizeValue').textContent = value + 'px';
});

// MISE À JOUR VISUELLE
function updateDots() {
    // Affiche les points seulement quand on commence à taper
    if (input.length > 0) {
        document.getElementById('codeDisplay').classList.add('active');
    } else {
        document.getElementById('codeDisplay').classList.remove('active');
    }
    
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
        e.stopPropagation();
        handleDigit(zone.dataset.digit);
    });
});

// Empêcher les comportements par défaut
document.addEventListener('gesturestart', (e) => {
    e.preventDefault();
});

document.addEventListener('touchmove', (e) => {
    if (document.body.classList.contains('locked')) {
        e.preventDefault();
    }
}, { passive: false });
</script>
</body>
</html>
