<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Jeu 2D Simple</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: #f0f0f0;
            margin: 0;
            font-family: sans-serif;
        }
        #game {
            position: relative;
            width: 600px;
            height: 200px;
            background: #fff;
            border: 2px solid #000;
            overflow: hidden;
        }
        #player {
            position: absolute;
            bottom: 0;
            left: 50px;
            width: 30px;
            height: 30px;
            background: red;
        }
        .obstacle {
            position: absolute;
            bottom: 0;
            width: 20px;
            height: 40px;
            background: green;
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
        <div id="player"></div>
        <div id="score">Score: 0</div>
    </div>

    <script>
        const player = document.getElementById('player');
        const game = document.getElementById('game');
        const scoreDisplay = document.getElementById('score');

        let playerBottom = 0;
        let isJumping = false;
        let score = 0;
        let obstacles = [];

        function jump() {
            if (isJumping) return;
            isJumping = true;
            let upInterval = setInterval(() => {
                if (playerBottom >= 100) {
                    clearInterval(upInterval);
                    let downInterval = setInterval(() => {
                        if (playerBottom <= 0) {
                            clearInterval(downInterval);
                            isJumping = false;
                        }
                        playerBottom -= 5;
                        player.style.bottom = playerBottom + 'px';
                    }, 20);
                }
                playerBottom += 5;
                player.style.bottom = playerBottom + 'px';
            }, 20);
        }

        function createObstacle() {
            const obstacle = document.createElement('div');
            obstacle.classList.add('obstacle');
            let obstaclePosition = 600;
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
                    obstaclePosition <= 80 &&
                    playerBottom < 40
                ) {
                    alert("Game Over! Score: " + score);
                    location.reload();
                }

                obstaclePosition -= 5;
                obstacle.style.right = obstaclePosition + 'px';
            }, 20);

            setTimeout(createObstacle, Math.random() * 3000 + 1000);
        }

        document.addEventListener('keydown', (e) => {
            if (e.code === 'Space') jump();
        });

        createObstacle();
    </script>
</body>
</html>
