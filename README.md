<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Игра Змейка</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100vh;
            background-color: #000;
            margin: 0;
        }
        canvas {
            border: 1px solid #fff;
        }
        .controls {
            display: flex;
            justify-content: center;
            margin-top: 10px;
        }
        .button {
            background-color: #fff;
            border: none;
            padding: 10px;
            margin: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <div class="controls">
        <button class="button" id="up">↑</button>
        <div>
            <button class="button" id="left">←</button>
            <button class="button" id="right">→</button>
        </div>
        <button class="button" id="down">↓</button>
    </div>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let snake = [{x: 10, y: 10}];
        let direction = {x: 0, y: 0};
        let food = {x: 5, y: 5};
        let score = 0;

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawSnake();
            drawFood();
            moveSnake();
            checkCollision();
        }

        function drawSnake() {
            ctx.fillStyle = 'green';
            for (let segment of snake) {
                ctx.fillRect(segment.x * 20, segment.y * 20, 18, 18);
            }
        }

        function drawFood() {
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x * 20, food.y * 20, 18, 18);
        }

        function moveSnake() {
            const head = {x: snake[0].x + direction.x, y: snake[0].y + direction.y};
            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score++;
                placeFood();
            } else {
                snake.pop();
            }
        }

        function placeFood() {
            food.x = Math.floor(Math.random() * (canvas.width / 20));
            food.y = Math.floor(Math.random() * (canvas.height / 20));
        }

        function checkCollision() {
            const head = snake[0];
            if (head.x < 0 || head.x >= canvas.width / 20 || head.y < 0 || head.y >= canvas.height / 20 || snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
                alert('Игра окончена! Ваш счет: ' + score);
                snake = [{x: 10, y: 10}];
                direction = {x: 0, y: 0};
                score = 0;
                placeFood();
            }
        }

        function changeDirection(newDirection) {
            if (newDirection.x !== -direction.x && newDirection.y !== -direction.y) {
                direction = newDirection;
            }
        }

        document.getElementById('up').onclick = () => changeDirection({x: 0, y: -1});
        document.getElementById('down').onclick = () => changeDirection({x: 0, y: 1});
        document.getElementById('left').onclick = () => changeDirection({x: -1, y: 0});
        document.getElementById('right').onclick = () => changeDirection({x: 1, y: 0});

        setInterval(draw, 100);
    </script>
</body>
</html>
