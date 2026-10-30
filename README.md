index.html
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kira Best Bingo - Telegram Ready</title>
    <!-- Telegram WebApp SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    <!-- Confetti Library for Winning Animation -->
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.5.1/dist/confetti.browser.min.js"></script>
    <style>
        body {
            background-color: #0b0f19;
            color: white;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 8px;
            text-align: center;
        }
        
        .top-nav-bar {
            background: linear-gradient(135deg, #1f1c2c, #2b5876);
            padding: 8px 10px;
            border-radius: 10px;
            margin-bottom: 8px;
            display: flex;
            flex-direction: column;
            gap: 6px;
            border: 1px solid #4ca1af;
        }
        .top-row-buttons {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .back-nav-btn {
            background: #d32f2f;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 11px;
            font-weight: bold;
            cursor: pointer;
        }
        .refresh-btn {
            background: #3f51b5;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 5px;
            font-size: 11px;
            font-weight: bold;
            cursor: pointer;
        }
        .wallet-info-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 4px;
        }
        .wallet-box {
            background: rgba(0, 0, 0, 0.3);
            padding: 5px;
            border-radius: 6px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            cursor: pointer;
            transition: 0.2s;
        }
        .wallet-box:active {
            transform: scale(0.96);
            background: rgba(0, 255, 204, 0.1);
        }
        .wallet-label {
            font-size: 9px;
            color: #a0aec0;
        }
        .wallet-value {
            font-size: 12px;
            font-weight: bold;
            color: #ffcc00;
            margin-top: 2px;
        }

        #selectionView {
            display: block;
        }
        .grid-container {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 4px;
            max-width: 440px;
            max-height: 38vh;
            overflow-y: auto;
            margin: 0 auto;
            padding: 5px;
            background: #070910;
            border-radius: 8px;
            border: 1px solid #1f1c2c;
        }
        .card-btn {
            background: linear-gradient(135deg, #3a6073, #16222a);
            color: white;
            border: 1px solid #4ca1af;
            padding: 9px 0;
            font-size: 12px;
            font-weight: bold;
            border-radius: 4px;
            cursor: pointer;
        }
        .card-btn.selected {
            background: linear-gradient(135deg, #ff416c, #ff4b2b) !important;
            border-color: #ffa07a !important;
        }

        .preview-container {
            display: flex;
            gap: 6px;
            justify-content: center;
            overflow-x: auto;
            margin-top: 8px;
            padding-bottom: 4px;
        }

        .start-game-btn {
            background: #4CAF50;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 14px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            margin-top: 8px;
            width: 100%;
            max-width: 440px;
        }

        #gameView {
            display: none;
            max-width: 440px;
            margin: 0 auto;
        }
        .called-banner {
            background: #1e1e38;
            padding: 6px 10px;
            border-radius: 8px;
            margin-bottom: 6px;
            border: 1px solid #00ffcc;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .big-number {
            font-size: 18px;
            font-weight: bold;
            color: #ffcc00;
            background: #0b0f19;
            padding: 3px 10px;
            border-radius: 6px;
            border: 1px solid #ffcc00;
        }
        
        .master-board {
            display: grid;
            grid-template-columns: repeat(15, 1fr);
            gap: 2px;
            background: #121629;
            padding: 4px;
            border-radius: 6px;
            margin-bottom: 6px;
        }
        .m-cell {
            background: #1a1f36;
            color: #aaa;
            font-size: 9px;
            padding: 3px 0;
            border-radius: 2px;
            text-align: center;
        }
        .m-cell.called {
            background: #ff5722;
            color: white;
            font-weight: bold;
        }

        .player-cards-container {
            display: flex;
            gap: 8px;
            justify-content: center;
            margin-bottom: 6px;
        }
        .bingo-card {
            background: #16162c;
            padding: 4px;
            border-radius: 6px;
            border: 2px solid #4ca1af;
            width: 48%;
        }
        .card-title {
            font-size: 11px;
            font-weight: bold;
            color: #00ffcc;
            margin-bottom: 3px;
        }
        .card-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 2px;
        }
        .b-header {
            background: #2196F3;
            color: white;
            padding: 3px 0;
            font-size: 10px;
            font-weight: bold;
            border-radius: 2px;
            text-align: center;
        }
        .b-cell {
            background: #2a2a4a;
            color: white;
            padding: 5px 0;
            font-size: 11px;
            font-weight: bold;
            border-radius: 2px;
            text-align: center;
        }
        .b-cell.marked {
            background: #4CAF50;
            color: white;
        }

        .bottom-action-bar {
            background: #121629;
            padding: 8px 12px;
            border-radius: 8px;
            display: flex;
            justify-content: space-around;
            align-items: center;
            border: 1px solid #333;
            margin-top: 4px;
        }
        .action-tab {
            display: flex;
            flex-direction: column;
            align-items: center;
            color: #a0aec0;
            font-size: 10px;
            cursor: pointer;
            background: none;
            border: none;
        }
        .action-tab.active {
            color: #00ffcc;
        }
        .action-tab span {
            font-size: 14px;
        }

        /* Win Modal */
        #winModal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.85);
            justify-content: center;
            align-items: center;
            z-index: 999;
        }
        .win-content {
            background: #1e1e2f;
            padding: 15px;
            border-radius: 14px;
            border: 1px solid #444;
            max-width: 320px;
            text-align: center;
            box-shadow: 0 10px 30px rgba(0,0,0,0.8);
        }
        .win-title {
            font-size: 20px;
            font-weight: bold;
            color: #ffcc00;
            margin-top: 4px;
        }
        .win-subtitle {
            font-size: 15px;
            font-weight: bold;
            color: #fff;
            margin: 6px 0 12px 0;
        }
        .modal-bingo-card {
            background: #2a2a3c;
            padding: 8px;
            border-radius: 10px;
            border: 1px solid #555;
            margin-bottom: 12px;
        }
        .modal-card-title {
            font-size: 13px;
            font-weight: bold;
            color: #ffcc00;
            margin-bottom: 6px;
        }
        .modal-card-grid {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 3px;
        }
        .modal-b-header {
            background: #2196F3;
            color: white;
            padding: 6px 0;
            font-size: 12px;
            font-weight: bold;
            border-radius: 3px;
            text-align: center;
        }
        .modal-b-cell {
            background: #ffffff;
            color: #000000;
            padding: 8px 0;
            font-size: 13px;
            font-weight: bold;
            border-radius: 3px;
            text-align: center;
        }
        .modal-b-cell.marked-normal {
            background: #4CAF50 !important;
            color: #ffffff !important;
        }
        .modal-b-cell.marked-winning {
            background: #ff9800 !important;
            color: #ffffff !important;
        }
        .auto-restart-badge {
            background: rgba(0, 0, 0, 0.4);
            color: #ffeb3b;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 12px;
            border: 1px solid #ffeb3b;
            display: inline-block;
        }

        /* Wallet Transfer Modal */
        #walletModal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.85);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .wallet-modal-content {
            background: #1a1e30;
            padding: 18px;
            border-radius: 14px;
            border: 1px solid #00ffcc;
            width: 85%;
            max-width: 320px;
            text-align: center;
        }
        .wallet-modal-title {
            font-size: 16px;
            font-weight: bold;
            color: #00ffcc;
            margin-bottom: 12px;
        }
        .transfer-box {
            background: #0f1220;
            padding: 10px;
            border-radius: 8px;
            margin-bottom: 10px;
            border: 1px solid #333;
        }
        .transfer-row {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            margin-bottom: 6px;
            color: #ccc;
        }
        .transfer-input {
            width: 90%;
            padding: 8px;
            background: #1f253d;
            border: 1px solid #4ca1af;
            color: white;
            border-radius: 6px;
            text-align: center;
            font-size: 14px;
            margin-bottom: 10px;
        }
        .transfer-btn {
            background: #00c853;
            color: white;
            border: none;
            padding: 9px 15px;
            font-size: 12px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            width: 48%;
        }
        .close-wallet-btn {
            background: #d32f2f;
            color: white;
            border: none;
            padding: 9px 15px;
            font-size: 12px;
            font-weight: bold;
            border-radius: 6px;
            cursor: pointer;
            width: 48%;
        }
    </style>
</head>
<body>

    <div class="top-nav-bar">
        <div class="top-row-buttons">
            <button class="back-nav-btn" onclick="goBackToSelection()">← Back</button>
            <div style="font-size: 13px; font-weight: bold; color: #00ffcc;" id="appTitle">🎮 Kira Best Bingo</div>
            <button class="refresh-btn" onclick="location.reload()">🔄 Refresh</button>
        </div>
        <div class="wallet-info-grid">
            <div class="wallet-box" onclick="openWalletModal('main')">
                <div class="wallet-label">Main Wallet</div>
                <div class="wallet-value" id="mainWalletDisplay">100 Br</div>
            </div>
            <div class="wallet-box" onclick="openWalletModal('play')">
                <div class="wallet-label">Play Wallet</div>
                <div class="wallet-value" id="playWalletDisplay">50 Br</div>
            </div>
            <div class="wallet-box">
                <div class="wallet-label">Stake</div>
                <div class="wallet-value" id="stakeDisplay">10 Br</div>
            </div>
            <div class="wallet-box">
                <div class="wallet-label">Timer</div>
                <div class="wallet-value" id="topTimerDisplay">60s</div>
            </div>
        </div>
    </div>

    <div id="selectionView">
        <p style="color: #a0aec0; font-size: 11px; margin: 2px 0;">ከ 1 እስከ 2 ካርዶችን ይምረጡ (Select 1 or 2 Cards):</p>
        <div class="grid-container" id="cardGrid"></div>
        <div class="preview-container" id="selectionPreviewContainer"></div>
        <button class="start-game-btn" onclick="startGameManually()">ጨዋታውን ጀምር (Start Game)</button>
    </div>

    <div id="gameView">
        <div class="called-banner">
            <div style="font-size: 11px; text-align: left;">የወጣው ቁጥር<br><span style="color:#aaa; font-size:9px;">Called Number</span></div>
            <div class="big-number" id="currentCalledNum">-</div>
        </div>

        <div style="font-size: 10px; color: #00ffcc; text-align: left; margin-bottom: 2px;">ማስተር ሰሌዳ (Master Board):</div>
        <div class="master-board" id="masterBoard"></div>

        <div class="player-cards-container" id="playerCardsContainer"></div>

        <div class="bottom-action-bar">
            <button class="action-tab active" onclick="goBackToSelection()"><span>🎮</span>Game</button>
            <button class="action-tab"><span>📜</span>History</button>
            <button class="action-tab" onclick="openWalletModal('main')"><span>💰</span>Wallet</button>
            <button class="action-tab"><span>👤</span>Profile</button>
        </div>
    </div>

    <div id="winModal">
        <div class="win-content">
            <div style="font-size: 28px;">👑</div>
            <div class="win-title">BINGO!</div>
            <div class="win-subtitle" id="winMessage">🎉 Kira WON! 🎉</div>
            
            <div class="modal-bingo-card">
                <div class="modal-card-title" id="modalCartelaTitle">🏆 Winning Cartela : 467</div>
                <div class="modal-card-grid" id="modalCardGrid"></div>
            </div>

            <div class="auto-restart-badge" id="autoRestartText">● Auto-starting next game in 5s</div>
        </div>
    </div>

    <!-- Wallet Transfer Modal Window -->
    <div id="walletModal">
        <div class="wallet-modal-content">
            <div class="wallet-modal-title">💰 የኪስ ቦት ዝውውር (Wallet Transfer)</div>
            <div class="transfer-box">
                <div class="transfer-row">
                    <span>Main Wallet:</span>
                    <strong style="color:#ffcc00;" id="mBalModal">100 Br</strong>
                </div>
                <div class="transfer-row">
                    <span>Play Wallet:</span>
                    <strong style="color:#00ffcc;" id="pBalModal">50 Br</strong>
                </div>
            </div>
            
            <p style="font-size: 11px; color: #aaa; margin: 4px 0;">የሚያዛውሩትን ብር መጠን ያስገቡ:</p>
            <input type="number" class="transfer-input" id="transferAmount" placeholder="መጠን (e.g. 20)" min="1">
            
            <div style="display: flex; justify-content: space-between; gap: 6px; margin-bottom: 10px;">
                <button class="transfer-btn" onclick="transferMoney('mainToPlay')">Main ➔ Play</button>
                <button class="transfer-btn" style="background: #00897b;" onclick="transferMoney('playToMain')">Play ➔ Main</button>
            </div>
            
            <button class="close-wallet-btn" style="width: 100%;" onclick="closeWalletModal()">ዘጋ (Close)</button>
        </div>
    </div>

    <script>
        // Telegram Web App Integration
        const tg = window.Telegram.WebApp;
        if (tg) {
            tg.expand();
            let user = tg.initDataUnsafe?.user;
            if (user && user.first_name) {
                document.getElementById('appTitle').innerText = `🎮 ${user.first_name}'s Bingo`;
            }
        }

        // Web Audio API for Click Sound
        const audioCtx = new (window.AudioContext || window.webkitAudioContext)();

        function playClickSound() {
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);
            let now = audioCtx.currentTime;
            osc.type = 'sine';
            osc.frequency.setValueAtTime(400, now);
            osc.frequency.exponentialRampToValueAtTime(800, now + 0.08);
            gain.gain.setValueAtTime(0.1, now);
            gain.gain.exponentialRampToValueAtTime(0.01, now + 0.08);
            osc.start(now);
            osc.stop(now + 0.08);
        }

        // English Voice Caller
        function speakEnglishNumber(num) {
            if ('speechSynthesis' in window) {
                let letter = '';
                if (num >= 1 && num <= 15) letter = 'B';
                else if (num >= 16 && num <= 30) letter = 'I';
                else if (num >= 31 && num <= 45) letter = 'N';
                else if (num >= 46 && num <= 60) letter = 'G';
                else if (num >= 61 && num <= 75) letter = 'O';

                let textToSpeak = `${letter} ${num}`;
                let utterance = new SpeechSynthesisUtterance(textToSpeak);
                utterance.lang = 'en-US';
                utterance.rate = 1.0;
                utterance.pitch = 1.1;
                
                window.speechSynthesis.speak(utterance);
            }
        }

        // Wallet Variables
        let mainBalance = 100;
        let playBalance = 50;
        let stakeAmount = 10;

        function updateWalletUI() {
            document.getElementById('mainWalletDisplay').innerText = mainBalance + ' Br';
            document.getElementById('playWalletDisplay').innerText = playBalance + ' Br';
            document.getElementById('stakeDisplay').innerText = stakeAmount + ' Br';
            document.getElementById('mBalModal').innerText = mainBalance + ' Br';
            document.getElementById('pBalModal').innerText = playBalance + ' Br';
        }

        function openWalletModal(type) {
            playClickSound();
            document.getElementById('walletModal').style.display = 'flex';
            updateWalletUI();
        }

        function closeWalletModal() {
            playClickSound();
            document.getElementById('walletModal').style.display = 'none';
        }

        function transferMoney(direction) {
            playClickSound();
            let amt = parseInt(document.getElementById('transferAmount').value);
            if (isNaN(amt) || amt <= 0) {
                alert('እባክዎ ትክክለኛ የብር መጠን ያስገቡ!');
                return;
            }

            if (direction === 'mainToPlay') {
                if (mainBalance < amt) {
                    alert('Main Wallet ውስጥ በቂ ሂሳብ የለም!');
                    return;
                }
                mainBalance -= amt;
                playBalance += amt;
            } else if (direction === 'playToMain') {
                if (playBalance < amt) {
                    alert('Play Wallet ውስጥ በቂ ሂሳብ የለም!');
                    return;
                }
                playBalance -= amt;
                mainBalance += amt;
            }

            document.getElementById('transferAmount').value = '';
            updateWalletUI();
            alert('ዝውውሩ በተሳካ ሁኔታ ተጠናቋል!');
        }

        const grid = document.getElementById('cardGrid');
        const selectionView = document.getElementById('selectionView');
        const gameView = document.getElementById('gameView');
        const topTimerDisplay = document.getElementById('topTimerDisplay');
        const masterBoard = document.getElementById('masterBoard');
        const playerCardsContainer = document.getElementById('playerCardsContainer');
        const selectionPreviewContainer = document.getElementById('selectionPreviewContainer');
        const currentCalledNum = document.getElementById('currentCalledNum');
        const winModal = document.getElementById('winModal');
        const winMessage = document.getElementById('winMessage');
        const modalCartelaTitle = document.getElementById('modalCartelaTitle');
        const modalCardGrid = document.getElementById('modalCardGrid');
        const autoRestartText = document.getElementById('autoRestartText');

        let selectedCards = [];
        let timeLeft = 60;
        let timerInterval;
        let callInterval;
        let gameActive = false;

        for (let i = 1; i <= 600; i++) {
            const btn = document.createElement('button');
            btn.className = 'card-btn';
            btn.innerText = i;
            btn.onclick = function() {
                playClickSound();
                toggleCardSelection(i, btn);
            };
            grid.appendChild(btn);
        }

        function toggleCardSelection(cardNum, btn) {
            const index = selectedCards.indexOf(cardNum);
            if (index > -1) {
                selectedCards.splice(index, 1);
                btn.classList.remove('selected');
            } else {
                if (selectedCards.length >= 2) {
                    alert('እባክዎ ቢበዛ 2 ካርዶች ብቻ ይምረጡ!');
                    return;
                }
                selectedCards.push(cardNum);
                btn.classList.add('selected');
            }
            updateSelectionPreview();
        }

        function updateSelectionPreview() {
            selectionPreviewContainer.innerHTML = '';
            if (selectedCards.length === 0) {
                selectionPreviewContainer.innerHTML = '<div style="color: #666; font-size: 11px; padding: 10px;">እባክዎ ካርድ ይምረጡ (No card selected)</div>';
                return;
            }

            selectedCards.forEach(cardNum => {
                const cardWrapper = document.createElement('div');
                cardWrapper.className = 'bingo-card';
                cardWrapper.style.width = '48%';

                const titleDiv = document.createElement('div');
                titleDiv.className = 'card-title';
                titleDiv.innerText = 'Cartela No : ' + cardNum;
                cardWrapper.appendChild(titleDiv);

                const cGrid = document.createElement('div');
                cGrid.className = 'card-grid';

                const letters = ['B', 'I', 'N', 'G', 'O'];
                letters.forEach(l => {
                    const lCell = document.createElement('div');
                    lCell.className = 'b-header';
                    lCell.innerText = l;
                    cGrid.appendChild(lCell);
                });

                let cardMatrix = generateStandardBingoCard(cardNum);

                for (let r = 0; r < 5; r++) {
                    for (let c = 0; c < 5; c++) {
                        const cell = document.createElement('div');
                        cell.className = 'b-cell';
                        if (r === 2 && c === 2) {
                            cell.innerText = '⭐';
                            cell.style.color = '#ffcc00';
                        } else {
                            cell.innerText = cardMatrix[r][c];
                        }
                        cGrid.appendChild(cell);
                    }
                }
                cardWrapper.appendChild(cGrid);
                selectionPreviewContainer.appendChild(cardWrapper);
            });
        }
        updateSelectionPreview();

        function startTimer() {
            timeLeft = 60;
            topTimerDisplay.innerText = timeLeft + 's';
            if (timerInterval) clearInterval(timerInterval);

            timerInterval = setInterval(() => {
                timeLeft--;
                topTimerDisplay.innerText = timeLeft + 's';
                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    if (selectedCards.length === 0) {
                        selectedCards.push(Math.floor(Math.random() * 600) + 1);
                    }
                    launchGame();
                }
            }, 1000);
        }
        startTimer();

        function startGameManually() {
            if (selectedCards.length === 0) {
                alert('እባክዎ ቢያንስ 1 ካርድ ይምረጡ!');
                return;
            }

            let totalCost = stakeAmount * selectedCards.length;
            if (playBalance < totalCost) {
                alert(`ለመጫወት በቂ ሂሳብ የሎትም! የሚያስፈልገው: ${totalCost} Br። እባክዎ ከ Main Wallet ይዛወሩ።`);
                openWalletModal('play');
                return;
            }

            playBalance -= totalCost;
            updateWalletUI();

            playClickSound();
            if (timerInterval) clearInterval(timerInterval);
            launchGame();
        }

        function launchGame() {
            selectionView.style.display = 'none';
            winModal.style.display = 'none';
            gameView.style.display = 'block';
            currentCalledNum.innerText = '-';
            gameActive = true;

            masterBoard.innerHTML = '';
            for (let i = 1; i <= 75; i++) {
                const mCell = document.createElement('div');
                mCell.className = 'm-cell';
                mCell.id = 'm-cell-' + i;
                mCell.innerText = i;
                masterBoard.appendChild(mCell);
            }

            playerCardsContainer.innerHTML = '';
            selectedCards.forEach(cardNum => {
                const cardWrapper = document.createElement('div');
                cardWrapper.className = 'bingo-card';
                cardWrapper.id = 'card-box-' + cardNum;

                const titleDiv = document.createElement('div');
                titleDiv.className = 'card-title';
                titleDiv.innerText = 'Cartela No : ' + cardNum;
                cardWrapper.appendChild(titleDiv);

                const cGrid = document.createElement('div');
                cGrid.className = 'card-grid';

                const letters = ['B', 'I', 'N', 'G', 'O'];
                letters.forEach(l => {
                    const lCell = document.createElement('div');
                    lCell.className = 'b-header';
                    lCell.innerText = l;
                    cGrid.appendChild(lCell);
                });

                let cardMatrix = generateStandardBingoCard(cardNum);

                for (let r = 0; r < 5; r++) {
                    for (let c = 0; c < 5; c++) {
                        const cell = document.createElement('div');
                        cell.className = 'b-cell';
                        if (r === 2 && c === 2) {
                            cell.innerText = '⭐';
                            cell.classList.add('marked', 'is-free');
                        } else {
                            let val = cardMatrix[r][c];
                            cell.innerText = val;
                            cell.dataset.val = val;
                        }
                        cGrid.appendChild(cell);
                    }
                }
                cardWrapper.appendChild(cGrid);
                playerCardsContainer.appendChild(cardWrapper);
            });

            startAutoCalling();
        }

        function generateStandardBingoCard(seed) {
            let colRanges = [
                {min: 1, max: 15},
                {min: 16, max: 30},
                {min: 31, max: 45},
                {min: 46, max: 60},
                {min: 61, max: 75}
            ];

            let cols = [];
            for (let c = 0; c < 5; c++) {
                let pool = [];
                for (let n = colRanges[c].min; n <= colRanges[c].max; n++) {
                    pool.push(n);
                }
                pool.sort(() => (Math.sin(seed++ + c) - 0.5));
                cols.push(pool.slice(0, 5));
            }

            let matrix = [];
            for (let r = 0; r < 5; r++) {
                let row = [];
                for (let c = 0; c < 5; c++) {
                    row.push(cols[c][r]);
                }
                matrix.push(row);
            }
            return matrix;
        }

        function startAutoCalling() {
            let availableNums = [];
            for (let i = 1; i <= 75; i++) availableNums.push(i);
            availableNums.sort(() => Math.random() - 0.5);

            let index = 0;
            if (callInterval) clearInterval(callInterval);

            callInterval = setInterval(() => {
                if (!gameActive) return;
                if (index >= availableNums.length) {
                    clearInterval(callInterval);
                    return;
                }
                let called = availableNums[index];
                index++;
                currentCalledNum.innerText = called;

                speakEnglishNumber(called);

                const mCell = document.getElementById('m-cell-' + called);
                if (mCell) mCell.classList.add('called');

                const allCells = playerCardsContainer.querySelectorAll('.b-cell');
                allCells.forEach(cell => {
                    if (cell.dataset.val == called) {
                        cell.classList.add('marked');
                    }
                });

                checkWinConditions();
            }, 2500);
        }

        function checkWinConditions() {
            if (!gameActive) return;

            selectedCards.forEach(cardNum => {
                const cardBox = document.getElementById('card-box-' + cardNum);
                if (!cardBox) return;

                const cells = cardBox.querySelectorAll('.b-cell');
                let matrix = [];
                for (let r = 0; r < 5; r++) {
                    let row = [];
                    for (let c = 0; c < 5; c++) {
                        let cell = cells[r * 5 + c];
                        if (r === 2 && c === 2) {
                            row.push(true);
                        } else {
                            row.push(cell.classList.contains('marked'));
                        }
                    }
                    matrix.push(row);
                }

                let winningCoords = [];

                for (let r = 0; r < 5; r++) {
                    if (matrix[r][0] && matrix[r][1] && matrix[r][2] && matrix[r][3] && matrix[r][4]) {
                        winningCoords = [[r,0], [r,1], [r,2], [r,3], [r,4]];
                    }
                }
                for (let c = 0; c < 5; c++) {
                    if (matrix[0][c] && matrix[1][c] && matrix[2][c] && matrix[3][c] && matrix[4][c]) {
                        winningCoords = [[0,c], [1,c], [2,c], [3,c], [4,c]];
                    }
                }
                if (matrix[0][0] && matrix[1][1] && matrix[2][2] && matrix[3][3] && matrix[4][4]) {
                    winningCoords = [[0,0], [1,1], [2,2], [3,3], [4,4]];
                }
                if (matrix[0][4] && matrix[1][3] && matrix[2][2] && matrix[3][1] && matrix[4][0]) {
                    winningCoords = [[0,4], [1,3], [2,2], [3,1], [4,0]];
                }
                if (matrix[0][0] && matrix[0][4] && matrix[4][0] && matrix[4][4]) {
                    winningCoords = [[0,0], [0,4], [4,0], [4,4]];
                }

                if (winningCoords.length > 0) {
                    triggerWin(cardNum, winningCoords);
                }
            });
        }

        function triggerWin(winningCardNum, winningCoords) {
            if (!gameActive) return;
            gameActive = false;
            if (callInterval) clearInterval(callInterval);

            let prizeAmount = stakeAmount * 4;
            playBalance += prizeAmount;
            updateWalletUI();

            if ('speechSynthesis' in window) {
                let winUtter = new SpeechSynthesisUtterance("Bingo! We have a winner!");
                winUtter.lang = 'en-US';
                window.speechSynthesis.speak(winUtter);
            }

            confetti({
                particleCount: 150,
                spread: 80,
                origin: { y: 0.6 }
            });

            winMessage.innerText = `🎉 Kira WON ${prizeAmount} Br! 🎉`;
            modalCartelaTitle.innerText = `🏆 Winning Cartela : ${winningCardNum}`;

            modalCardGrid.innerHTML = '';
            const letters = ['B', 'I', 'N', 'G', 'O'];
            letters.forEach(l => {
                const lCell = document.createElement('div');
                lCell.className = 'modal-b-header';
                lCell.innerText = l;
                modalCardGrid.appendChild(lCell);
            });

            const cardBox = document.getElementById('card-box-' + winningCardNum);
            const originalCells = cardBox.querySelectorAll('.b-cell');

            originalCells.forEach((origCell, index) => {
                const r = Math.floor(index / 5);
                const c = index % 5;

                const mCell = document.createElement('div');
                mCell.className = 'modal-b-cell';
                mCell.innerText = origCell.innerText;

                if (origCell.classList.contains('marked')) {
                    let isWinningCell = winningCoords.some(coord => coord[0] === r && coord[1] === c);
                    
                    if (isWinningCell) {
                        mCell.classList.add('marked-winning');
                    } else {
                        mCell.classList.add('marked-normal');
                    }
                }
                modalCardGrid.appendChild(mCell);
            });

            winModal.style.display = 'flex';

            let countdown = 5;
            autoRestartText.innerText = `● Auto-starting next game in ${countdown}s`;

            let restartInterval = setInterval(() => {
                countdown--;
                autoRestartText.innerText = `● Auto-starting next game in ${countdown}s`;
                if (countdown <= 0) {
                    clearInterval(restartInterval);
                    goBackToSelection();
                }
            }, 1000);
        }

        function goBackToSelection() {
            playClickSound();
            if (callInterval) clearInterval(callInterval);
            if (timerInterval) clearInterval(timerInterval);
            gameActive = false;
            winModal.style.display = 'none';
            gameView.style.display = 'none';
            selectionView.style.display = 'block';
            selectedCards = [];
            
            const allCardBtns = grid.querySelectorAll('.card-btn');
            allCardBtns.forEach(b => b.classList.remove('selected'));
            updateSelectionPreview();
            startTimer();
        }

        updateWalletUI();
    </script>

</body>
</html>
