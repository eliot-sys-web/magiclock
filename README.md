<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone Lock</title>

<!-- Plein écran sur iPhone -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

<style>
/* SUPPRESSION DU SCROLL */
html, body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    touch-action: manipulation;
}

/* CONTENEUR PRINCIPAL IPHONE 13 */
#phone {
    width: 100%;
    height: 100%;
    max-width: 390px;   /* largeur iPhone 13 */
    max-height: 844px;  /* hauteur iPhone 13 */
    margin: auto;
    position: relative;

    /* IMAGE EN FOND : PLEIN ÉCRAN, ENTIER VISIBLE */
    background-image: url("lockscreen.png");
    background-size: contain;       /* aucune partie coupée */
    background-position: center;
    background-repeat: no-repeat;
}

/* ÉCRAN DÉVERROUILLÉ */
#unlocked {
    display: none;
    position: absolute;
    inset: 0;
    background-image: url("IMG_3629.png");
    background-size: contain;
    background-position: center;
    background-repeat: no-repeat;
}

/* MESSAGE SECRET DISCRET */
#secret {
    position: absolute;
    bottom: 10px;
    right: 12px;
    font-size: 11px;
    opacity: 0.35;
    color: white;
    font-family: -apple-system, BlinkMacSystemFont;
}

/* ZONES TACTILES INVISIBLES */
.touch {
    position: absolute;
    opacity: 0;
}
</style>
</head>

<body>
<div id="phone">

    <!-- ZONES CLIQUABLES POUR LES CHIFFRES - iPhone 13 -->
    <div class="touch" data="1" style="left:45px;  top:515px; width:80px; height:80px;"></div>
    <div class="touch" data="2" style="left:155px; top:515px; width:80px; height:80px;"></div>
    <div class="touch" data="3" style="left:265px; top:515px; width:80px; height:80px;"></div>

    <div class="touch" data="4" style="left:45px;  top:610px; width:80px; height:80px;"></div>
    <div class="touch" data="5" style="left:155px; top:610px; width:80px; height:80px;"></div>
    <div class="touch" data="6" style="left:265px; top:610px; width:80px; height:80px;"></div>

    <div class="touch" data="7" style="left:45px;  top:705px; width:80px; height:80px;"></div>
    <div class="touch" data="8" style="left:155px; top:705px; width:80px; height:80px;"></div>
    <div class="touch" data="9" style="left:265px; top:705px; width:80px; height:80px;"></div>

    <div class="touch" data="0" style="left:155px; top:795px; width:80px; height:80px;"></div>

    <!-- ÉCRAN DÉVERROUILLÉ -->
    <div id="unlocked">
        <div id="secret"></div>
    </div>
</div>

<script>
// 🔐 CODES VALIDES
const VALID_CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

let input = "";
let firstWrong = null;

function reset() {
    input = "";
}

function unlock(celebrity) {
    document.getElementById("unlocked").style.display = "block";
    document.getElementById("secret").innerText =
        celebrity + " • " + firstWrong;
}

function handleDigit(d) {
    if (input.length >= 6) return;

    input += d;

    if (input.length === 6) {
        setTimeout(() => {
            if (!firstWrong) {
                firstWrong = input;   // mémorise le 1er code
                reset();
            } else if (VALID_CODES[input]) {
                unlock(VALID_CODES[input]);
            } else {
                reset();
            }
        }, 250);
    }
}

// ASSOCIATION DES ZONES CLIQUABLES
document.querySelectorAll(".touch").forEach(zone => {
    zone.addEventListener("click", () => handleDigit(zone.dataset.data));
});
</script>
</body>
</html>
>
