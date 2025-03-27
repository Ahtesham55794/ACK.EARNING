<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aviator Betting Game</title>
    <style>
        body {
            text-align: center;
            font-family: Arial, sans-serif;
            background-color: #222;
            color: white;
        }
        canvas {
            background-color: black;
            border: 2px solid white;
        }
        button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <h1>Aviator Betting Game</h1>
    <p>بیٹ لگائیں اور کریش ہونے سے پہلے کیش آؤٹ کریں!</p>
    
    <canvas id="gameCanvas" width="400" height="500"></canvas>
    <br>
    <button id="betButton">بیٹ لگائیں</button>
    <button id="cashoutButton" disabled>کیش آؤٹ</button>
    
    <p id="balance">بیلنس: 100</p>
    <p id="multiplier">ملٹیپلائر: 1.00x</p>
    <p id="status">گیم شروع ہونے کا انتظار...</p>

    <script>
        let balance = 100;
        let betAmount = 10;
        let multiplier = 1.00;
        let isBetting = false;
        let isCashedOut = false;
        let crashPoint;
        let gameInterval;

        const balanceDisplay = document.getElementById("balance");
        const multiplierDisplay = document.getElementById("multiplier");
        const statusDisplay = document.getElementById("status");
        const betButton = document.getElementById("betButton");
        const cashoutButton = document.getElementById("cashoutButton");
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        function startGame() {
            crashPoint = Math.random() * 5 + 1; // 1x to 6x
            multiplier = 1.00;
            isCashedOut = false;
            isBetting = true;
            balance -= betAmount;
            balanceDisplay.textContent = "بیلنس: " + balance;
            betButton.disabled = true;
            cashoutButton.disabled = false;
            statusDisplay.textContent = "گیم جاری ہے...";
            gameLoop();
        }

        function gameLoop() {
            gameInterval = setInterval(() => {
                multiplier += 0.1;
                multiplierDisplay.textContent = "ملٹیپلائر: " + multiplier.toFixed(2) + "x";
                
                drawPlane(multiplier * 40); // راکٹ کو اوپر لے جانے کے لیے

                if (multiplier >= crashPoint) {
                    clearInterval(gameInterval);
                    isBetting = false;
                    statusDisplay.textContent = "کریش ہوگیا! 😢";
                    betButton.disabled = false;
                    cashoutButton.disabled = true;
                }
            }, 300);
        }

        function cashOut() {
            if (isBetting && !isCashedOut) {
                clearInterval(gameInterval);
                balance += betAmount * multiplier;
                balanceDisplay.textContent = "بیلنس: " + balance;
                statusDisplay.textContent = "آپ نے " + multiplier.toFixed(2) + "x پر کیش آؤٹ کیا! 🎉";
                isCashedOut = true;
                isBetting = false;
                betButton.disabled = false;
                cashoutButton.disabled = true;
            }
        }

        function drawPlane(y) {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            ctx.fillStyle = "red";
            ctx.fillRect(180, canvas.height - y, 40, 40); // راکٹ
        }

        betButton.addEventListener("click", startGame);
        cashoutButton.addEventListener("click", cashOut);
    </script>

</body>
</html>
