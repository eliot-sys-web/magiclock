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
}

#container {
    width: 100%;
    height: 100%;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
}

#phone {
    width: 100vw;
    height: 100vh;
    position: relative;
    background-image: url("Img_3630.png");
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
    background-image: url("IMG_3629.png");
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
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
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
    font-family: -apple-system;
    font-size: 12px;
    z-index: 1000;
    cursor: pointer;
}

/* MESSAGE D'ERREUR DISCRET */
#errorShake {
    position: absolute;
    top: 420px;
    left: 50%;
    transform: translateX(-50%);
    color: white;
    font-size: 13px;
    font-family: -apple-system;
    opacity: 0;
    transition: opacity 0.3s;
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
<div id="container">
    <div id="phone">
        <!-- AFFICHAGE DU CODE (6 points) -->
        <div id="codeDisplay">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
        
        <div id="errorShake"></div>

        <!-- ZONES CLIQUABLES (iPhone 13 - 390x844) -->
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

        <!-- ÉCRAN DÉVERROUILLÉ -->
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

const dots = document.querySelectorAll('.dot');
const phone = document.getElementById('phone');

// MODE CALIBRATION (appui long sur l'écran)
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

// MISE À JOUR VISUELLE DES POINTS
function updateDots() {
    dots.forEach((dot, i) => {
        if (i < input.length) {
            dot.classList.add('filled');
        } else {
            dot.classList.remove('filled');
        }
    });
}

// RESET
function reset() {
    input = "";
    updateDots();
}

// ANIMATION D'ERREUR
function showError() {
    phone.classList.add('shake');
    setTimeout(() => {
        phone.classList.remove('shake');
        reset();
    }, 400);
}

// DÉVERROUILLAGE
function unlock(celebrity) {
    isUnlocked = true;
    document.getElementById("unlocked").style.display = "block";
    document.getElementById("secret").innerHTML = 
        `${celebrity}<br>${firstCode}`;
    
    // Vibration de succès
    if (navigator.vibrate) {
        navigator.vibrate([50, 100, 50]);
    }
}

// GESTION DES CHIFFRES
function handleDigit(digit) {
    if (isUnlocked || calibrationMode) return;
    if (input.length >= 6) return;
    
    // Vibration légère
    if (navigator.vibrate) {
        navigator.vibrate(10);
    }
    
    input += digit;
    updateDots();
    
    // Vérification quand 6 chiffres
    if (input.length === 6) {
        setTimeout(() => {
            if (!firstCode) {
                // Premier code : on mémorise et reset
                firstCode = input;
                reset();
            } else if (VALID_CODES[input]) {
                // Deuxième code valide : déverrouillage
                unlock(VALID_CODES[input]);
            } else {
                // Deuxième code invalide : erreur
                showError();
            }
        }, 300);
    }
}

// ASSOCIATION DES ZONES TACTILES
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

// Empêcher le zoom
document.addEventListener('gesturestart', (e) => {
    e.preventDefault();
});
</script>
</body>
</html>
