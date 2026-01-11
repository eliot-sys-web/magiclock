# magiclock
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>iPhone Lock</title>
<style>
body {
    margin: 0;
    background: black;
    font-family: -apple-system, BlinkMacSystemFont, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}
#phone {
    width: 320px;
    height: 640px;
    border-radius: 40px;
    background: black;
    overflow: hidden;
    color: white;
    position: relative;
}
#lockscreen {
    text-align: center;
    padding-top: 60px;
}
#lock-icon {
    font-size: 28px;
    margin-bottom: 10px;
}
#title {
    font-size: 18px;
    margin-bottom: 25px;
}
#dots {
    display: flex;
    justify-content: center;
    margin-bottom: 30px;
}
.dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    border: 1px solid white;
    margin: 6px;
}
.filled {
    background: white;
}
#keypad {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 14px;
    padding: 0 30px;
}
.key {
    border: 1px solid #666;
    border-radius: 50%;
    height: 65px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-size: 22px;
    cursor: pointer;
}
.key span {
    font-size: 10px;
    letter-spacing: 1px;
    opacity: 0.8;
}
#bottom {
    display: flex;
    justify-content: space-between;
    padding: 20px 30px;
    font-size: 14px;
    opacity: 0.8;
}
#unlocked {
    display: none;
    width: 100%;
    height: 100%;
    position: relative;
}
#unlocked img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
#secret {
    position: absolute;
    bottom: 8px;
    right: 10px;
    font-size: 11px;
    opacity: 0.35;
    color: white;
}
</style>
</head>

<body>
<div id="phone">

    <!-- ÉCRAN VERROUILLÉ -->
    <div id="lockscreen">
        <div id="lock-icon">🔒</div>
        <div id="title">Face ID ou code</div>

        <div id="dots"></div>

        <div id="keypad"></div>

        <div id="bottom">
            <div>Urgences</div>
            <div>Annuler</div>
        </div>
    </div>

    <!-- ÉCRAN DÉVERROUILLÉ -->
    <div id="unlocked">
        <img src="photo.jpg" alt="Écran déverrouillé">
        <div id="secret"></div>
    </div>

</div>

<script>
// 🔑 Codes valides + célébrités
const VALID_CODES = {
    "123456": "Brad Pitt",
    "654321": "Emma Watson",
    "111111": "Leonardo DiCaprio"
};

let input = "";
let firstWrongCode = null;

const dots = document.getElementById("dots");
const keypad = document.getElementById("keypad");

function updateDots() {
    dots.innerHTML = "";
    for (let i = 0; i < 6; i++) {
        const d = document.createElement("div");
        d.className = "dot" + (i < input.length ? " filled" : "");
        dots.appendChild(d);
    }
}

function resetInput() {
    input = "";
    updateDots();
}

function unlock(celebrity) {
    document.getElementById("lockscreen").style.display = "none";
    document.getElementById("unlocked").style.display = "block";
    document.getElementById("secret").innerText =
        `${celebrity} • ${firstWrongCode}`;
}

function handleDigit(d) {
    if (input.length >= 6) return;
    input += d;
    updateDots();

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

// Création du clavier façon iPhone
const layout = [
    ["1",""],["2","ABC"],["3","DEF"],
    ["4","GHI"],["5","JKL"],["6","MNO"],
    ["7","PQRS"],["8","TUV"],["9","WXYZ"],
    ["",""],["0",""],["",""]
];

layout.forEach(k => {
    const key = document.createElement("div");
    key.className = "key";
    if (k[0]) {
        key.innerHTML = k[0] + (k[1] ? `<span>${k[1]}</span>` : "");
        key.onclick = () => handleDigit(k[0]);
    } else {
        key.style.border = "none";
    }
    keypad.appendChild(key);
});

updateDots();
</script>

</body>
</html>
