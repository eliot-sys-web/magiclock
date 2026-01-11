<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone</title>
<!-- PWA CONFIGURATION COMPLÈTE -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no, maximum-scale=1.0, minimum-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="">
<meta name="mobile-web-app-capable" content="yes">
<link rel="apple-touch-icon" href="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVR42mNk+M9QDwADhgGAWjR9awAAAABJRU5ErkJggg==">
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
    touch-action: none;
    position: fixed;
    height: 100%;
    width: 100%;
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
    background-size: 100% 100%;
    background-position: center center;
    background-repeat: no-repeat;
}

/* ZONES TACTILES - INVISIBLES MAIS CLIQUABLES */
.touch {
    position: absolute;
    z-index: 10;
    cursor: pointer;
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

/* BOUTON SETTINGS - Caché par défaut */
#settingsBtn {
    position: fixed;
    top: 20px;
    left: 20px;
    width: 50px;
    height: 50px;
    background: rgba(0,0,0,0.8);
    color: white;
    border: 2px solid rgba(255,255,255,0.5);
    border-radius: 50%;
    font-size: 24px;
    z-index: 100000;
    cursor: pointer;
    display: none;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 15px rgba(0,0,0,0.8);
}

#settingsBtn.visible {
    display: flex;
}

#settingsMenu {
    display: none;
    position: fixed;
    top: 80px;
    left: 20px;
    background: rgba(0,0,0,0.95);
    border: 1px solid rgba(255,255,255,0.3);
    border-radius: 12px;
    padding: 15px;
    z-index: 100001;
    color: white;
    font-size: 13px;
    min-width: 200px;
}

#settingsMenu.visible {
    display: block;
}

.menu-option {
    padding: 10px;
    border-bottom: 1px solid rgba(255,255,255,0.1);
    cursor: pointer;
}

.menu-option:last-child {
    border-bottom: none;
}

.menu-option:active {
    background: rgba(255,255,255,0.1);
}

/* MODE CALIBRATION */
body.calibration .touch {
    background: rgba(255,0,0,0.4);
    border: 2px solid red;
}

/* Instructions de calibration */
#calibInfo {
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
    z-index: 100002;
    text-align: center;
    max-width: 80%;
    line-height: 1.5;
}

body.calibration #calibInfo {
    display: block;
}

/* Boutons de sauvegarde en mode calibration */
#calibButtons {
    display: none;
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    z-index: 100003;
    gap: 10px;
}

body.calibration #calibButtons {
    display: flex;
}

#calibButtons button {
    padding: 12px 24px;
    border: none;
    border-radius: 8px;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
}

#saveCalib {
    background: #4CAF50;
    color: white;
}

#cancelCalib {
    background: rgba(255,255,255,0.2);
    color: white;
}

#exportCalib {
    background: #2196F3;
    color: white;
}

/* Instruction d'appui long */
#longPressHint {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0,0,0,0.9);
    color: white;
    padding: 20px 30px;
    border-radius: 16px;
    font-size: 14px;
    z-index: 100003;
    text-align: center;
    display: none;
    animation: fadeInOut 3s forwards;
}

@keyframes fadeInOut {
    0% { opacity: 0; }
    10% { opacity: 1; }
    90% { opacity: 1; }
    100% { opacity: 0; display: none; }
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

<!-- ÉCRAN DE CONFIGURATION -->
<div id="setup">
    <h1>📱 iPhone Lock Screen</h1>
    <p>Crée ton écran de verrouillage personnalisé</p>
    
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
        💡 <strong>Astuce :</strong> Tes images seront sauvegardées automatiquement. Pour ajouter cette app à ton écran d'accueil : partage → Sur l'écran d'accueil.
    </div>
</div>

<!-- ÉCRAN PRINCIPAL -->
<div id="container">
    <div id="phone">
        <!-- Zones tactiles (positions en %) -->
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
</div>

<!-- BOUTON SETTINGS -->
<button id="settingsBtn">⚙️</button>
<div id="settingsMenu">
    <div class="menu-option" onclick="toggleCalibrationMode()">🎯 Voir les zones tactiles</div>
    <div class="menu-option" onclick="changeImages()">🖼️ Changer les images</div>
    <div class="menu-option" onclick="resetAll()">🗑️ Tout réinitialiser</div>
</div>

<div id="calibInfo">
    🎯 <strong>Mode déplacement activé</strong><br>
    Glisse les zones rouges pour les positionner<br>
    Clique sur "Sauvegarder" quand c'est OK
</div>

<div id="calibButtons">
    <button id="cancelCalib" onclick="cancelCalibration()">Annuler</button>
    <button id="exportCalib" onclick="exportPositions()">📋 Copier</button>
    <button id="saveCalib" onclick="saveCalibration()">✓ Sauvegarder</button>
</div>

<div id="longPressHint">
    💡 Appui long pour les réglages
</div>

<script>
// 🔐 CODES VALIDES
const VALID_CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

// STOCKAGE LOCAL
const STORAGE_KEYS = {
    lockscreen: 'iphone_lock_lockscreen',
    homescreen: 'iphone_lock_homescreen'
};

let input = "";
let firstCode = null;
let isUnlocked = false;
let calibrationMode = false;
let longPressTimer = null;
let settingsVisible = false;

// Pour le glisser-déposer des zones
let draggedElement = null;
let originalPositions = {};
let isDragging = false;

const phone = document.getElementById('phone');
const setup = document.getElementById('setup');
const container = document.getElementById('container');
const startBtn = document.getElementById('startBtn');
const settingsBtn = document.getElementById('settingsBtn');

// CHARGEMENT DES IMAGES SAUVEGARDÉES
window.addEventListener('load', function() {
    const savedLock = localStorage.getItem(STORAGE_KEYS.lockscreen);
    const savedHome = localStorage.getItem(STORAGE_KEYS.homescreen);
    
    if (savedLock && savedHome) {
        // Images déjà sauvegardées : démarrer directement
        phone.style.backgroundImage = `url(${savedLock})`;
        document.getElementById('unlocked').style.backgroundImage = `url(${savedHome})`;
        setup.style.display = 'none';
        container.classList.add('visible');
        document.body.classList.add('locked');
        
        // Afficher le hint après 2 secondes
        setTimeout(() => {
            document.getElementById('longPressHint').style.display = 'block';
        }, 2000);
    }
});

// GESTION DES UPLOADS
document.getElementById('lockscreen').addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
            const data = event.target.result;
            localStorage.setItem(STORAGE_KEYS.lockscreen, data);
            document.getElementById('lockPreview').src = data;
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
            const data = event.target.result;
            localStorage.setItem(STORAGE_KEYS.homescreen, data);
            document.getElementById('homePreview').src = data;
            document.getElementById('homePreview').classList.add('visible');
            checkReady();
        };
        reader.readAsDataURL(file);
    }
});

function checkReady() {
    const hasLock = localStorage.getItem(STORAGE_KEYS.lockscreen);
    const hasHome = localStorage.getItem(STORAGE_KEYS.homescreen);
    if (hasLock && hasHome) {
        startBtn.classList.add('visible');
    }
}

startBtn.addEventListener('click', function() {
    const lockData = localStorage.getItem(STORAGE_KEYS.lockscreen);
    const homeData = localStorage.getItem(STORAGE_KEYS.homescreen);
    
    phone.style.backgroundImage = `url(${lockData})`;
    document.getElementById('unlocked').style.backgroundImage = `url(${homeData})`;
    setup.style.display = 'none';
    container.classList.add('visible');
    document.body.classList.add('locked');
    
    // Afficher le hint après 2 secondes
    setTimeout(() => {
        document.getElementById('longPressHint').style.display = 'block';
    }, 2000);
});

// APPUI LONG pour afficher les réglages - Sur TOUT le container
// Fonctionne sur tactile ET souris
let longPressStartTime = 0;

function handleLongPressStart(e) {
    if (isUnlocked) return;
    
    // Ne pas déclencher l'appui long si on est sur une zone en mode calibration
    if (calibrationMode && e.target.classList.contains('touch')) {
        return;
    }
    
    longPressStartTime = Date.now();
    
    longPressTimer = setTimeout(() => {
        // Vibration
        if (navigator.vibrate) {
            navigator.vibrate(50);
        }
        
        if (!settingsVisible) {
            // Afficher le bouton settings
            settingsBtn.classList.add('visible');
            settingsVisible = true;
        } else {
            // Toggle du mode calibration
            calibrationMode = !calibrationMode;
            if (calibrationMode) {
                document.body.classList.add('calibration');
                // Sauvegarder les positions originales
                document.querySelectorAll('.touch').forEach(zone => {
                    originalPositions[zone.dataset.digit] = {
                        left: zone.style.left,
                        top: zone.style.top
                    };
                });
            } else {
                document.body.classList.remove('calibration');
            }
        }
    }, 1500); // 1.5 secondes
}

function handleLongPressEnd(e) {
    const pressDuration = Date.now() - longPressStartTime;
    clearTimeout(longPressTimer);
}

function handleLongPressCancel(e) {
    // Ne pas annuler si on drag une zone
    if (!isDragging) {
        clearTimeout(longPressTimer);
    }
}

// Tactile
container.addEventListener('touchstart', handleLongPressStart);
container.addEventListener('touchend', handleLongPressEnd);
container.addEventListener('touchmove', handleLongPressCancel);

// Souris (pour tester sur ordinateur)
container.addEventListener('mousedown', handleLongPressStart);
container.addEventListener('mouseup', handleLongPressEnd);
container.addEventListener('mousemove', handleLongPressCancel);

// MENU SETTINGS
settingsBtn.addEventListener('click', function(e) {
    e.stopPropagation();
    const menu = document.getElementById('settingsMenu');
    menu.classList.toggle('visible');
});

document.addEventListener('click', function() {
    document.getElementById('settingsMenu').classList.remove('visible');
});

function toggleCalibrationMode() {
    calibrationMode = !calibrationMode;
    if (calibrationMode) {
        document.body.classList.add('calibration');
        // Sauvegarder les positions originales
        document.querySelectorAll('.touch').forEach(zone => {
            originalPositions[zone.dataset.digit] = {
                left: zone.style.left,
                top: zone.style.top
            };
        });
    } else {
        document.body.classList.remove('calibration');
    }
    document.getElementById('settingsMenu').classList.remove('visible');
}

function saveCalibration() {
    // Sauvegarder les positions dans localStorage
    const positions = {};
    document.querySelectorAll('.touch').forEach(zone => {
        positions[zone.dataset.digit] = {
            left: zone.style.left,
            top: zone.style.top,
            width: zone.style.width,
            height: zone.style.height
        };
    });
    localStorage.setItem('zone_positions', JSON.stringify(positions));
    
    calibrationMode = false;
    document.body.classList.remove('calibration');
    
    alert('✓ Positions sauvegardées !');
}

function cancelCalibration() {
    // Restaurer les positions originales
    document.querySelectorAll('.touch').forEach(zone => {
        if (originalPositions[zone.dataset.digit]) {
            zone.style.left = originalPositions[zone.dataset.digit].left;
            zone.style.top = originalPositions[zone.dataset.digit].top;
        }
    });
    
    calibrationMode = false;
    document.body.classList.remove('calibration');
}

function exportPositions() {
    const positions = {};
    document.querySelectorAll('.touch').forEach(zone => {
        positions[zone.dataset.digit] = {
            left: zone.style.left,
            top: zone.style.top
        };
    });
    
    const text = JSON.stringify(positions, null, 2);
    
    // Copier dans le presse-papier
    if (navigator.clipboard) {
        navigator.clipboard.writeText(text).then(() => {
            alert('📋 Positions copiées dans le presse-papier !');
        });
    } else {
        alert('Positions:\n' + text);
    }
}

function changeImages() {
    settingsVisible = false;
    settingsBtn.classList.remove('visible');
    document.body.classList.remove('locked');
    container.classList.remove('visible');
    setup.style.display = 'flex';
    document.getElementById('settingsMenu').classList.remove('visible');
    
    // Charger les images actuelles dans les previews
    const lockData = localStorage.getItem(STORAGE_KEYS.lockscreen);
    const homeData = localStorage.getItem(STORAGE_KEYS.homescreen);
    if (lockData) {
        document.getElementById('lockPreview').src = lockData;
        document.getElementById('lockPreview').classList.add('visible');
    }
    if (homeData) {
        document.getElementById('homePreview').src = homeData;
        document.getElementById('homePreview').classList.add('visible');
    }
    checkReady();
}

function resetAll() {
    if (confirm('Supprimer toutes les données et recommencer ?')) {
        localStorage.removeItem(STORAGE_KEYS.lockscreen);
        localStorage.removeItem(STORAGE_KEYS.homescreen);
        location.reload();
    }
}

// LOGIQUE DU CODE
function reset() {
    input = "";
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

// ZONES TACTILES - Glisser-déposer en mode calibration
document.querySelectorAll(".touch").forEach(zone => {
    let touchStartTime = 0;
    let startX, startY, offsetX, offsetY;
    
    // Fonction de démarrage du drag
    function startDrag(e, clientX, clientY) {
        if (!calibrationMode) {
            touchStartTime = Date.now();
            return;
        }
        
        isDragging = true;
        draggedElement = zone;
        zone.classList.add('dragging');
        
        const rect = zone.getBoundingClientRect();
        const containerRect = phone.getBoundingClientRect();
        
        const offsetX = clientX - rect.left;
        const offsetY = clientY - rect.top;
        
        // Stocker les offsets sur l'élément
        zone._offsetX = offsetX;
        zone._offsetY = offsetY;
        
        e.preventDefault();
    }
    
    // Fonction de déplacement
    function doDrag(e, clientX, clientY) {
        if (!isDragging || !calibrationMode) return;
        
        const containerRect = phone.getBoundingClientRect();
        
        let newLeft = ((clientX - containerRect.left - offsetX) / containerRect.width) * 100;
        let newTop = ((clientY - containerRect.top - offsetY) / containerRect.height) * 100;
        
        // Limiter aux bords
        newLeft = Math.max(0, Math.min(newLeft, 100 - parseFloat(zone.style.width)));
        newTop = Math.max(0, Math.min(newTop, 100 - parseFloat(zone.style.height)));
        
        zone.style.left = newLeft + '%';
        zone.style.top = newTop + '%';
        
        e.preventDefault();
    }
    
    // Fonction de fin du drag
    function endDrag(e) {
        if (isDragging) {
            isDragging = false;
            zone.classList.remove('dragging');
            draggedElement = null;
        } else if (!calibrationMode) {
            const duration = Date.now() - touchStartTime;
            if (duration < 500) {
                handleDigit(zone.dataset.digit);
            }
        }
    }
    
    // Événements tactiles
    zone.addEventListener("touchstart", function(e) {
        const touch = e.touches[0];
        startDrag(e, touch.clientX, touch.clientY);
    });
    
    zone.addEventListener("touchmove", function(e) {
        const touch = e.touches[0];
        doDrag(e, touch.clientX, touch.clientY);
    });
    
    zone.addEventListener("touchend", endDrag);
    
    // Événements souris
    zone.addEventListener("mousedown", function(e) {
        startDrag(e, e.clientX, e.clientY);
        e.stopPropagation(); // Empêcher la propagation au container
    });
    
    zone.addEventListener("mousemove", function(e) {
        if (draggedElement === zone) {
            doDrag(e, e.clientX, e.clientY);
        }
    });
    
    zone.addEventListener("mouseup", function(e) {
        if (draggedElement === zone) {
            endDrag(e);
            e.stopPropagation();
        }
    });
});

// Charger les positions sauvegardées au démarrage
const savedPositions = localStorage.getItem('zone_positions');
if (savedPositions) {
    const positions = JSON.parse(savedPositions);
    document.querySelectorAll('.touch').forEach(zone => {
        const digit = zone.dataset.digit;
        if (positions[digit]) {
            zone.style.left = positions[digit].left;
            zone.style.top = positions[digit].top;
        }
    });
}

// Ajouter les événements globaux pour le mousemove et mouseup
document.addEventListener("mousemove", function(e) {
    if (draggedElement && calibrationMode) {
        const containerRect = phone.getBoundingClientRect();
        
        let newLeft = ((e.clientX - containerRect.left - draggedElement._offsetX) / containerRect.width) * 100;
        let newTop = ((e.clientY - containerRect.top - draggedElement._offsetY) / containerRect.height) * 100;
        
        newLeft = Math.max(0, Math.min(newLeft, 100 - parseFloat(draggedElement.style.width)));
        newTop = Math.max(0, Math.min(newTop, 100 - parseFloat(draggedElement.style.height)));
        
        draggedElement.style.left = newLeft + '%';
        draggedElement.style.top = newTop + '%';
        
        e.preventDefault();
    }
});

document.addEventListener("mouseup", function(e) {
    if (draggedElement && calibrationMode) {
        draggedElement.classList.remove('dragging');
        draggedElement = null;
        isDragging = false;
    }
});

// Empêcher les comportements par défaut
document.addEventListener('gesturestart', function(e) {
    e.preventDefault();
});

document.addEventListener('touchmove', function(e) {
    if (document.body.classList.contains('locked')) {
        e.preventDefault();
    }
}, { passive: false });
</script>
</body>
</html>
