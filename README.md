<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Jeu du Chien</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            font-family: sans-serif;
        }
        #game {
            position: relative;
            width: 800px;
            height: 200px;
            background-color: #fff;
            border: 2px solid #000;
            overflow: hidden;
        }
        #dog {
            position: absolute;
            bottom: 0;
            left: 50px;
            width: 40px;
            height: 40px;
            background-color: brown;
            border-radius: 50%;
        }
        .obstacle {
            position: absolute;
            bottom: 0;
            width: 20px;
            height: 40px;
            background-color: green;
            right: 0;
        }
        #score {
            position: absolute;
            top: 5px;
            left: 5px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div id="game">
        <div id="dog"></div>
        <div id="score">Score: 0</div>
    </div>
<script>
        const dog = document.getElementById('dog');
        const game = document.getElementById('game');
        const scoreDisplay = document.getElementById('score');
         let dogBottom = 0;
        let isJumping = false;
        let score = 0;
        function jump() {
            if (isJumping) return;
            isJumping = true;
            let upInterval = setInterval(() => {
                if (dogBottom >= 100) {
                    clearInterval(upInterval);
                    let downInterval = setInterval(() => {
                        if (dogBottom <= 0) {
                            clearInterval(downInterval);
                            isJumping = false;
                        }
                        dogBottom -= 5;
                        dog.style.bottom = dogBottom + 'px';
                    }, 20);
                }
                dogBottom += 5;
                dog.style.bottom = dogBottom + 'px';
            }, 20);
        }
        function createObstacle() {
            const obstacle = document.createElement('div');
            obstacle.classList.add('obstacle');
            let obstaclePosition = 800;
            game.appendChild(obstacle);
            let moveInterval = setInterval(() => {
                if (obstaclePosition < -20) {
                    clearInterval(moveInterval);
                    game.removeChild(obstacle);
                    score += 1;
                    scoreDisplay.textContent = "Score: " + score;
                }
                // Collision detection
                if (
                    obstaclePosition >= 50 &&
                    obstaclePosition <= 90 &&
                    dogBottom < 40
                ) {
                    alert("Game Over! Score: " + score);
                    location.reload();
                }
                obstaclePosition -= 6;
                obstacle.style.right = obstaclePosition + 'px';
            }, 20);
            // Next obstacle random time
            setTimeout(createObstacle, Math.random() * 3000 + 1000);
        }
        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space') jump();
        });
        createObstacle();
    </script>
</body>
</html>
