<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone Lock</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<style>
html, body {
    margin: 0;
    padding: 0;
    height: 100%;
    width: 100%;
    overflow: hidden;
    touch-action: manipulation;
}

#phone {
    width: 100%;
    height: 100%;
    max-width: 390px; /* iPhone 13 width */
    max-height: 844px; /* iPhone 13 height */
    margin: auto;
    position: relative;
}

#phone img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}

#unlocked {
    display: none;
    position: absolute;
    top:0; left:0;
    width:100%; height:100%;
}

#secret {
    position: absolute;
    bottom: 8px;
    right: 10px;
    font-size: 11px;
    opacity: 0.35;
    color: white;
}

.touch-area {
    position: absolute;
    opacity: 0;
}
</style>
</head>
<body>
<div id="phone">
    <!-- IMAGE DE TON ÉCRAN DE VERROUILLAGE -->
    <img src="lockscreen.png" alt="Écran verrouillé">

    <!-- ZONES CLIQUABLES pour chaque chiffre sur iPhone 13 -->
    <!-- Ajusté pour clavier standard 1-9 / 0 -->
    <div class="touch-area" data-digit="1" style="left:50px; top:520px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="2" style="left:160px; top:520px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="3" style="left:270px; top:520px; width:70px; height:70px;"></div>

    <div class="touch-area" data-digit="4" style="left:50px; top:610px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="5" style="left:160px; top:610px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="6" style="left:270px; top:610px; width:70px; height:70px;"></div>

    <div class="touch-area" data-digit="7" style="left:50px; top:700px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="8" style="left:160px; top:700px; width:70px; height:70px;"></div>
    <div class="touch-area" data-digit="9" style="left:270px; top:700px; width:70px; height:70px;"></div>

    <div class="touch-area" data-digit="0" style="left:160px; top:790px; width:70px; height:70px;"></div>

    <!-- ÉCRAN DÉVERROUILLÉ -->
    <div id="unlocked">
        <img src="IMG_3629.png" alt="Écran déverrouillé">
        <div id="secret"></div>
    </div>
</div>

<script>
// Codes valides + célébrités
const VALID_CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

let input = "";
let firstWrongCode = null;

function unlock(celebrity) {
    document.getElementById("unlocked").style.display = "block";
    document.getElementById("secret").innerText =
        `${celebrity} • ${firstWrongCode}`;
}

function resetInput() {
    input = "";
}

function handleDigit(d) {
    if (input.length >= 6) return;
    input += d;

    if (input.length === 6) {
        setTimeout(() => {
            if (!firstWrongCode) {
                firstWrongCode = input;
                resetInput();
            } else if (VALID_CODES[input]) {
                unlock(VALID_CODES[input]);
            } else {
                resetInput();
            }
        }, 300);
    }
}

// Associer les zones cliquables
document.querySelectorAll(".touch-area").forEach(el => {
    el.addEventListener("click", () => handleDigit(el.dataset.digit));
});
</script>
</body>
</html>
