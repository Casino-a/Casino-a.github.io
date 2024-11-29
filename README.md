
<html lang="de">
<head>
    <meta charset="UTF-8">
    <title>Premium Casino Slot Machine</title>
    <style>
        body {
            margin: 0;
            min-height: 100vh;
            background: linear-gradient(135deg, #1a1a1a, #0a0a2e);
            font-family: Arial, sans-serif;
            color: white;
        }

        .casino-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .slot-machine {
            text-align: center;
            padding: 30px;
            background: linear-gradient(45deg, #2c3e50, #3498db);
            border-radius: 20px;
            box-shadow: 0 0 30px rgba(0,0,0,0.5);
            margin: 20px auto;
            border: 3px solid gold;
        }

        .money-display {
            font-size: 24px;
            color: gold;
            text-shadow: 0 0 10px rgba(255,215,0,0.5);
            margin: 20px 0;
            background: rgba(0,0,0,0.3);
            padding: 15px;
            border-radius: 10px;
            border: 2px solid rgba(255,215,0,0.3);
        }

        .slots {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin: 30px 0;
        }

        .slot {
            width: 150px;
            height: 150px;
            background: rgba(255,255,255,0.1);
            border: 5px solid gold;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
            box-shadow: 0 0 20px rgba(255,215,0,0.3);
            transition: transform 0.3s;
        }

        .slot.spinning {
            animation: spin 0.2s linear infinite;
        }

        @keyframes spin {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        .controls {
            display: flex;
            gap: 20px;
            justify-content: center;
            margin: 20px 0;
        }

        button {
            padding: 15px 30px;
            font-size: 20px;
            background: linear-gradient(45deg, #ffd700, #ffa500);
            border: none;
            border-radius: 10px;
            cursor: pointer;
            transition: 0.3s;
            color: #000;
            font-weight: bold;
            text-transform: uppercase;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }

        button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0,0,0,0.3);
        }

        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        #resetButton {
            margin-top: 20px;
            background: linear-gradient(45deg, #ff4444, #cc0000);
            color: white;
        }

        #resetButton:hover {
            background: linear-gradient(45deg, #ff6666, #ff0000);
        }

        #result {
            font-size: 24px;
            margin: 20px 0;
            padding: 15px;
            border-radius: 10px;
            transition: 0.3s;
        }

        .win {
            background: rgba(0,255,0,0.2);
            border: 2px solid #00ff00;
        }

        .lose {
            background: rgba(255,0,0,0.2);
            border: 2px solid #ff0000;
        }

        @media (max-width: 768px) {
            .slots {
                gap: 10px;
            }
            .slot {
                width: 100px;
                height: 100px;
                font-size: 40px;
            }
        }

        @media (max-width: 480px) {
            .slot {
                width: 80px;
                height: 80px;
                font-size: 30px;
            }
            .controls {
                flex-direction: column;
                align-items: center;
            }
        }
    </style>
</head>
<body>
    <div class="casino-container">
        <div class="slot-machine">
            <h1>🎰 Premium Casino 🎰</h1>
            <div class="money-display">
                Guthaben: <span id="balance">1000</span> €
            </div>
            <div class="slots">
                <div class="slot" id="slot1">🎰</div>
                <div class="slot" id="slot2">🎰</div>
                <div class="slot" id="slot3">🎰</div>
            </div>
            <div class="controls">
                <button onclick="setBet(10)">10€ Setzen</button>
                <button onclick="setBet(50)">50€ Setzen</button>
                <button onclick="setBet(100)">100€ Setzen</button>
            </div>
            <button id="spinButton" onclick="spin()">Drehen</button>
            <button id="resetButton" onclick="resetBalance()">Neues Spiel</button>
            <p id="result"></p>
        </div>
    </div>

    <script>
        const symbols = ['💎', '7️⃣', '🎰', '🍒', '🍊', '🍋'];
        let balance = parseInt(localStorage.getItem('casinoBalance')) || 1000;
        let currentBet = 10;
        let isSpinning = false;

        function updateBalance() {
            document.getElementById('balance').textContent = balance;
            localStorage.setItem('casinoBalance', balance);
            
            if (balance < 10) {
                document.getElementById('spinButton').disabled = true;
                document.getElementById('result').textContent = '😢 Du hast leider kein Geld mehr! Starte ein neues Spiel! 😢';
                document.getElementById('result').className = 'lose';
            } else {
                document.getElementById('spinButton').disabled = false;
            }
        }

        function resetBalance() {
            if(confirm('Möchtest du wirklich ein neues Spiel starten?')) {
                balance = 1000;
                updateBalance();
                document.getElementById('result').textContent = '🎮 Neues Spiel gestartet! Viel Glück! 🎮';
                document.getElementById('result').className = '';
            }
        }

        function setBet(amount) {
            if (!isSpinning && balance >= amount) {
                currentBet = amount;
                alert(`Einsatz auf ${amount}€ gesetzt!`);
            } else if (balance < amount) {
                alert('Nicht genügend Guthaben für diesen Einsatz!');
            }
        }

        function calculateWin(results) {
            if (results[0] === results[1] && results[1] === results[2]) {
                if (results[0] === '💎') return currentBet * 10;
                if (results[0] === '7️⃣') return currentBet * 7;
                return currentBet * 5;
            }
            if (results[0] === results[1] || results[1] === results[2] || results[0] === results[2]) {
                return currentBet * 2;
            }
            return 0;
        }

        function spin() {
            if (isSpinning || balance < currentBet) return;
            
            isSpinning = true;
            balance -= currentBet;
            updateBalance();
            
            const slots = document.querySelectorAll('.slot');
            const results = [];
            let completedSlots = 0;
            
            slots.forEach((slot, index) => {
                slot.classList.add('spinning');
                
                setTimeout(() => {
                    const randomIndex = Math.floor(Math.random() * symbols.length);
                    const symbol = symbols[randomIndex];
                    results[index] = symbol;
                    
                    let count = 0;
                    const interval = setInterval(() => {
                        slot.textContent = symbols[Math.floor(Math.random() * symbols.length)];
                        count++;
                        if (count > 20) {
                            clearInterval(interval);
                            slot.textContent = symbol;
                            slot.classList.remove('spinning');
                            completedSlots++;
                            
                            if (completedSlots === 3) {
                                const winAmount = calculateWin(results);
                                const resultElement = document.getElementById('result');
                                
                                if (winAmount > 0) {
                                    balance += winAmount;
                                    resultElement.textContent = `🎉 Gewonnen! +${winAmount}€ 🎉`;
                                    resultElement.className = 'win';
                                } else {
                                    resultElement.textContent = '😢 Verloren! Versuchen Sie es noch einmal! 😢';
                                    resultElement.className = 'lose';
                                }
                                
                                updateBalance();
                                isSpinning = false;
                            }
                        }
                    }, 50);
                }, index * 500);
            });
        }

        // Initialisiere Balance beim Laden
        updateBalance();
    </script>
</body>
</html>
