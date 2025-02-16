<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Футбольный Вратарь</title>
    <style>
        body { text-align: center; font-family: Arial, sans-serif; }
        canvas { display: block; margin: auto; border: 2px solid black; background: url('https://upload.wikimedia.org/wikipedia/commons/6/66/Soccer_field_-_empty.png') no-repeat center/cover; }
    </style>
</head>
<body>
    <h1>Защити ворота!</h1>
    <canvas id="gameCanvas" width="800" height="500"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const goalKeeper = { x: 375, y: 450, width: 50, height: 50 };
        const ball = { x: Math.random() * 760 + 20, y: 0, radius: 15, speedY: 5 };
        const goalImage = new Image();
        const keeperImage = new Image();
        let imagesLoaded = 0;

        function imageLoaded() {
            imagesLoaded++;
            if (imagesLoaded === 2) {
                gameLoop();
            }
        }

        goalImage.onload = imageLoaded;
        keeperImage.onload = imageLoaded;
        goalImage.src = 'https://upload.wikimedia.org/wikipedia/commons/thumb/8/88/Football_goal.svg/800px-Football_goal.svg.png';
        keeperImage.src = 'https://upload.wikimedia.org/wikipedia/commons/2/2c/Goalkeeper.svg';

        canvas.addEventListener("mousemove", (event) => {
            const rect = canvas.getBoundingClientRect();
            goalKeeper.x = event.clientX - rect.left - goalKeeper.width / 2;
        });

        function drawGoal() {
            ctx.drawImage(goalImage, 300, 400, 200, 100);
        }

        function drawGoalKeeper() {
            ctx.drawImage(keeperImage, goalKeeper.x, goalKeeper.y, goalKeeper.width, goalKeeper.height);
        }

        function drawBall() {
            ctx.fillStyle = "white";
            ctx.beginPath();
            ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
            ctx.fill();
        }

        function update() {
            ball.y += ball.speedY;
            
            if (
                ball.y + ball.radius >= goalKeeper.y &&
                ball.x >= goalKeeper.x &&
                ball.x <= goalKeeper.x + goalKeeper.width
            ) {
                ball.y = 0;
                ball.x = Math.random() * 760 + 20;
            }
            
            if (ball.y > canvas.height) {
                alert("Ты пропустил гол!");
                ball.y = 0;
                ball.x = Math.random() * 760 + 20;
            }
        }

        function gameLoop() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawGoal();
            drawGoalKeeper();
            drawBall();
            update();
            requestAnimationFrame(gameLoop);
        }
    </script>
</body>
</html>
