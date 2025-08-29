# Questions pour un Champion - ENSPM Maroua

<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Questions pour un Champion - ENSPM Maroua</title>
<style>
    body {
        margin: 0;
        font-family: Arial, sans-serif;
        display: flex;
        justify-content: center;
        align-items: center;
        min-height: 100vh;
        background: #f0f0f0;
    }
    #game {
        background: #fff;
        padding: 20px 30px;
        border-radius: 10px;
        box-shadow: 0 0 10px rgba(0,0,0,0.3);
        max-width: 600px;
        width: 100%;
    }
    #question {
        font-size: 20px;
        margin-bottom: 20px;
    }
    .option {
        display: block;
        background: #e0e0e0;
        padding: 10px 15px;
        margin-bottom: 10px;
        border-radius: 5px;
        cursor: pointer;
        transition: 0.2s;
        border: none;
        text-align: left;
    }
    .option:hover {
        background: #c0c0c0;
    }
    .correct {
        background-color: #4CAF50 !important; /* vert */
        color: #fff;
    }
    .wrong {
        background-color: #f44336 !important; /* rouge */
        color: #fff;
    }
    #nextBtn, #replayBtn {
        padding: 10px 20px;
        font-size: 16px;
        margin-top: 15px;
        cursor: pointer;
        border: none;
        border-radius: 5px;
        background-color: #4CAF50;
        color: #fff;
    }
    #score {
        font-weight: bold;
        margin-bottom: 10px;
    }
    #replayBtn {
        display: none;
        background-color: orange;
    }
</style>
</head>
<body>
<div id="game">
    <div id="score">Score: 0</div>
    <div id="question"></div>
    <div id="options"></div>
    <button id="nextBtn">Suivant</button>
    <button id="replayBtn">Rejouer</button>
</div>

<script>
// Questions ENSPM Maroua
const questions = [
    {
        question: "Quelle est la couleur du logo de l'ENSPM?",
        options: ["Bleu", "Vert", "Rouge", "Jaune"],
        answer: 0
    },
    {
        question: "En quelle année l'ENSPM a-t-elle été créée?",
        options: ["2001", "1995", "2010", "1988"],
        answer: 1
    },
    {
        question: "Quel département est spécialisé en génie informatique?",
        options: ["DEG", "DES", "DIG", "DEP"],
        answer: 2
    },
    {
        question: "Combien d'années dure la formation en master?",
        options: ["1", "2", "3", "4"],
        answer: 1
    }
];

let currentQuestion = 0;
let score = 0;

// Afficher la question et options
function showQuestion() {
    const q = questions[currentQuestion];
    document.getElementById("question").innerText = q.question;
    const optionsDiv = document.getElementById("options");
    optionsDiv.innerHTML = "";

    q.options.forEach((opt, index) => {
        const btn = document.createElement("button");
        btn.classList.add("option");
        btn.innerText = opt;
        btn.addEventListener("click", () => selectAnswer(index, btn));
        optionsDiv.appendChild(btn);
    });
}

function selectAnswer(index, button) {
    const q = questions[currentQuestion];

    // Coloration des boutons
    const optionButtons = document.querySelectorAll(".option");
    optionButtons.forEach((btn, i) => {
        btn.disabled = true;
        if(i === q.answer){
            btn.classList.add("correct");
        } else if(i === index){
            btn.classList.add("wrong");
        }
    });

    // Incrémenter score si correct
    if(index === q.answer){
        score++;
        document.getElementById("score").innerText = "Score: " + score;
    }
}

// Passer à la question suivante
document.getElementById("nextBtn").addEventListener("click", () => {
    currentQuestion++;
    if(currentQuestion >= questions.length) {
        endGame();
    } else {
        showQuestion();
    }
});

function endGame() {
    document.getElementById("question").innerText = "Jeu terminé! Ton score final est: " + score;
    document.getElementById("options").innerHTML = "";
    document.getElementById("nextBtn").style.display = "none";
    document.getElementById("replayBtn").style.display = "block";
}

// Rejouer
document.getElementById("replayBtn").addEventListener("click", () => {
    currentQuestion = 0;
    score = 0;
    document.getElementById("score").innerText = "Score: 0";
    document.getElementById("nextBtn").style.display = "block";
    document.getElementById("replayBtn").style.display = "none";
    showQuestion();
});

// Lancer la première question
showQuestion();
</script>
</body>
</html>
