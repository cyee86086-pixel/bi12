<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>X/O PRO - الذكاء الاصطناعي المتقدم</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            min-height: 100vh;
            background: linear-gradient(135deg, #2a0845, #6441a5);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Poppins', system-ui, -apple-system, sans-serif;
            padding: 20px;
        }

        /* الحاوية الرئيسية */
        .game-container {
            background: rgba(46, 12, 68, 0.7);
            backdrop-filter: blur(12px);
            border-radius: 64px;
            padding: 25px 30px 35px;
            box-shadow: 0 25px 45px rgba(0, 0, 0, 0.5), inset 0 1px 2px rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            transition: all 0.3s ease;
        }

        /* واجهة البداية */
        .start-screen, .game-screen {
            width: 100%;
            transition: 0.4s cubic-bezier(0.2, 0.9, 0.4, 1.1);
        }

        .start-screen {
            text-align: center;
            min-width: 320px;
        }

        .game-screen {
            display: none;
            flex-direction: column;
            align-items: center;
        }

        h1 {
            font-size: 3.2rem;
            background: linear-gradient(135deg, #ff9a9e, #fad0c4, #fbc2eb);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-shadow: 0 0 15px rgba(0,0,0,0.3);
            margin-bottom: 20px;
            letter-spacing: 3px;
        }

        .sub {
            color: #e0b3ff;
            margin-bottom: 40px;
            font-weight: 500;
        }

        /* حقول الإدخال */
        .input-group {
            background: rgba(0,0,0,0.4);
            border-radius: 60px;
            padding: 5px 20px;
            margin: 15px 0;
            display: flex;
            align-items: center;
            gap: 12px;
            backdrop-filter: blur(4px);
        }

        .input-group label {
            font-weight: bold;
            color: #ffd966;
            width: 80px;
            text-align: left;
        }

        .input-group input, .input-group select {
            flex: 1;
            padding: 12px 15px;
            border-radius: 40px;
            border: none;
            background: #2e1b42;
            color: white;
            font-size: 1rem;
            outline: none;
            transition: 0.2s;
        }

        .input-group input:focus, .input-group select:focus {
            box-shadow: 0 0 0 2px #b77eff;
        }

        .btn-start {
            background: linear-gradient(95deg, #c45eff, #7a2be0);
            border: none;
            padding: 14px 30px;
            font-size: 1.4rem;
            font-weight: bold;
            color: white;
            border-radius: 50px;
            cursor: pointer;
            margin-top: 25px;
            transition: 0.2s;
            width: 100%;
            letter-spacing: 2px;
        }

        .btn-start:hover {
            transform: scale(1.02);
            box-shadow: 0 8px 20px rgba(0,0,0,0.4);
            filter: brightness(1.05);
        }

        /* لوحة اللعب */
        .status {
            font-size: 1.5rem;
            background: #2a0f3a;
            padding: 12px 25px;
            border-radius: 50px;
            margin-bottom: 20px;
            color: #ffecb3;
            font-weight: bold;
            text-align: center;
            width: 100%;
        }

        .board {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-template-rows: repeat(3, 100px);
            gap: 12px;
            background: rgba(0,0,0,0.3);
            padding: 15px;
            border-radius: 48px;
            margin-bottom: 25px;
        }

        .cell {
            background: linear-gradient(145deg, #351c4a, #241536);
            border-radius: 28px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 4rem;
            font-weight: 800;
            color: white;
            text-shadow: 0 5px 10px rgba(0,0,0,0.4);
            cursor: pointer;
            transition: all 0.1s ease;
            box-shadow: 0 8px 0 #12081c;
            transform: translateY(-2px);
            position: relative;
        }

        /* تأثير السقوط عند الضغط */
        .cell.fall-animation {
            animation: cellDrop 0.2s cubic-bezier(0.34, 1.2, 0.64, 1) forwards;
        }

        @keyframes cellDrop {
            0% { transform: translateY(-15px) scale(0.8); opacity: 0; }
            80% { transform: translateY(4px) scale(1.02); }
            100% { transform: translateY(0px) scale(1); opacity: 1; }
        }

        .cell:active {
            transform: scale(0.96);
        }

        /* علامات X و O بألوان مميزة */
        .cell.X {
            color: #ff6b82;
            text-shadow: 0 0 8px #ff3366;
        }
        .cell.O {
            color: #6bcbff;
            text-shadow: 0 0 8px #2a9eff;
        }

        .reset-btn {
            background: #ffb347;
            border: none;
            padding: 10px 25px;
            font-size: 1.2rem;
            font-weight: bold;
            border-radius: 40px;
            cursor: pointer;
            margin-top: 8px;
            transition: 0.2s;
            color: #2c0b3a;
        }

        .reset-btn:hover {
            background: #ff9f1a;
            transform: scale(0.97);
        }

        .back-menu {
            background: rgba(255,255,240,0.2);
            border: none;
            padding: 8px 18px;
            border-radius: 30px;
            margin-top: 12px;
            color: white;
            cursor: pointer;
        }

        /* تأثير اهتزاز للخسارة/الربح */
        .shake-effect {
            animation: shake 0.3s ease-in-out 0s 2;
        }

        @keyframes shake {
            0%,100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }

        footer {
            font-size: 0.7rem;
            text-align: center;
            margin-top: 20px;
            opacity: 0.7;
            color: #d9b4ff;
        }
    </style>
</head>
<body>
<div class="game-container" id="gameContainer">
    <!-- شاشة البداية -->
    <div class="start-screen" id="startScreen">
        <h1>⚡ X/O PRO ⚡</h1>
        <div class="sub">ذكاء اصطناعي متطور بأربعة مستويات</div>
        <div class="input-group">
            <label>👤 اسمك:</label>
            <input type="text" id="playerName" placeholder="البطل" maxlength="15" value="محارب">
        </div>
        <div class="input-group">
            <label>🤖 مستوى AI:</label>
            <select id="aiLevel">
                <option value="weak">🟢 ضعيف</option>
                <option value="normal">🟡 طبيعي</option>
                <option value="smart">🔵 ذكي</option>
                <option value="pro">🔴 محترف</option>
            </select>
        </div>
        <button class="btn-start" id="beginBtn">🚀 ابدأ المباراة 🚀</button>
        <footer>X/O PRO - تحدَّ الذكاء الاصطناعي</footer>
    </div>

    <!-- شاشة اللعب -->
    <div class="game-screen" id="gameScreen">
        <div class="status" id="statusMsg">جاهز للانطلاق!</div>
        <div class="board" id="board">
            <!-- 9 خلايا سيتم إنشاؤها بالجافاسكريبت -->
        </div>
        <button class="reset-btn" id="resetGame">🔄 إعادة اللعبة</button>
        <button class="back-menu" id="backToMenu">🏠 القائمة الرئيسية</button>
        <footer style="margin-top:12px">X/O PRO - أنيميشن سقوط + صوت</footer>
    </div>
</div>

<script>
    // ----- عناصر DOM -----
    const startScreen = document.getElementById('startScreen');
    const gameScreen = document.getElementById('gameScreen');
    const beginBtn = document.getElementById('beginBtn');
    const resetBtn = document.getElementById('resetGame');
    const backMenuBtn = document.getElementById('backToMenu');
    const playerNameInput = document.getElementById('playerName');
    const aiLevelSelect = document.getElementById('aiLevel');
    const statusDiv = document.getElementById('statusMsg');
    const boardDiv = document.getElementById('board');

    // ----- متغيرات اللعبة -----
    let boardState = ['', '', '', '', '', '', '', '', ''];
    let currentPlayer = 'X';     // X دائمًا الإنسان (يبدأ أولاً)
    let gameActive = true;
    let playerName = 'محارب';
    let aiLevel = 'normal';
    let aiSymbol = 'O';
    let humanSymbol = 'X';
    
    // مؤثرات صوتية (باستخدام Web Audio API لنبضات حادة)
    let audioCtx = null;
    function initAudio() {
        if (!audioCtx) {
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        }
    }
    // صوت النقر (سقوط الخانة)
    function playDropSound() {
        if (!audioCtx) return;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.frequency.value = 880;
        gain.gain.value = 0.2;
        osc.start();
        gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 0.3);
        osc.stop(audioCtx.currentTime + 0.3);
    }
    // صوت فوز/خسارة حاد ونبرة مختلفة
    function playWinLoseSound(isWin, isPlayerWin) {
        if (!audioCtx) return;
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();
        osc.connect(gain);
        gain.connect(audioCtx.destination);
        if (isWin) {
            // نغمة انتصار سريعة جميلة
            osc.frequency.value = isPlayerWin ? 1200 : 800;
            gain.gain.value = 0.25;
            osc.start();
            gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 0.5);
            osc.stop(audioCtx.currentTime + 0.5);
            // نغمة ثانية للفوز
            if(isPlayerWin){
                const osc2 = audioCtx.createOscillator();
                const gain2 = audioCtx.createGain();
                osc2.connect(gain2);
                gain2.connect(audioCtx.destination);
                osc2.frequency.value = 1500;
                gain2.gain.value = 0.15;
                osc2.start(audioCtx.currentTime + 0.1);
                gain2.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 0.55);
                osc2.stop(audioCtx.currentTime + 0.55);
            }
        } else {
            // صوت تعادل أو خسارة
            osc.frequency.value = 440;
            gain.gain.value = 0.2;
            osc.start();
            gain.gain.exponentialRampToValueAtTime(0.00001, audioCtx.currentTime + 0.4);
            osc.stop(audioCtx.currentTime + 0.4);
        }
    }
    
    // تأثير اهتزاز الحاوية
    function shakeContainer() {
        const cont = document.getElementById('gameContainer');
        cont.classList.add('shake-effect');
        setTimeout(() => {
            cont.classList.remove('shake-effect');
        }, 400);
    }
    
    // تحديث واجهة الحالة مع اسم اللاعب
    function updateStatus(message, isSpecial = false) {
        statusDiv.innerText = message;
        if(isSpecial && (message.includes('ربح') || message.includes('خسر') || message.includes('فوز'))) {
            // ننادي الاهتزاز
            shakeContainer();
        }
    }
    
    // عرض الخانات مع تأثير
    function renderBoard() {
        const cells = document.querySelectorAll('.cell');
        for (let i = 0; i < cells.length; i++) {
            cells[i].innerText = boardState[i];
            if(boardState[i] === 'X') {
                cells[i].classList.add('X');
                cells[i].classList.remove('O');
            } else if(boardState[i] === 'O') {
                cells[i].classList.add('O');
                cells[i].classList.remove('X');
            } else {
                cells[i].classList.remove('X','O');
            }
        }
    }
    
    // فوز + نهاية اللعبة
    function checkWinner(board, symbol) {
        const winPatterns = [
            [0,1,2], [3,4,5], [6,7,8],
            [0,3,6], [1,4,7], [2,5,8],
            [0,4,8], [2,4,6]
        ];
        return winPatterns.some(pattern => pattern.every(idx => board[idx] === symbol));
    }
    
    function isDraw(board) {
        return board.every(cell => cell !== '');
    }
    
    function endGame(winner) {
        gameActive = false;
        let msg = '';
        let isWinEvent = false;
        let playerWin = false;
        if(winner === 'X') {
            msg = `🎉 ${playerName} ربح المباراة! 🎉`;
            playerWin = true;
            isWinEvent = true;
            playWinLoseSound(true, true);
            updateStatus(msg, true);
            // نطق باسم اللاعب بنبرة حادة (باستخدام الكلام)
            speakText(`${playerName} فزت! رائع جداً!`);
        } else if(winner === 'O') {
            msg = `🤖 الذكاء الاصطناعي خَسرك! حاول مجدداً.`;
            playerWin = false;
            isWinEvent = true;
            playWinLoseSound(true, false);
            updateStatus(msg, true);
            speakText(`للأسف ${playerName} خسرت أمام الذكاء الاصطناعي`);
        } else {
            msg = `🤝 تعادل! مستوى رائع يا ${playerName}`;
            playWinLoseSound(false, false);
            updateStatus(msg, true);
            speakText(`تعادل، مستوى ممتاز يا ${playerName}`);
        }
        if(isWinEvent && winner) statusDiv.innerText = msg;
        else if(!winner && isDraw(boardState)) statusDiv.innerText = msg;
    }
    
    // تحويل الحركة مع صوت و انيميشن
    function makeMove(index, symbol) {
        if(!gameActive) return false;
        if(boardState[index] !== '') return false;
        if(currentPlayer !== symbol) return false;
        
        // تطبيق الحركة
        boardState[index] = symbol;
        renderBoard();
        playDropSound();
        // تأثير السقوط على الخلية
        const cellDiv = document.querySelector(`.cell[data-idx='${index}']`);
        if(cellDiv) {
            cellDiv.classList.remove('fall-animation');
            void cellDiv.offsetWidth; // إعادة التدفق
            cellDiv.classList.add('fall-animation');
            setTimeout(() => cellDiv.classList.remove('fall-animation'), 250);
        }
        
        // فحص الفوز
        if(checkWinner(boardState, symbol)) {
            endGame(symbol);
            return true;
        }
        if(isDraw(boardState)) {
            endGame(null);
            return true;
        }
        // تبديل اللاعب
        currentPlayer = (currentPlayer === 'X') ? 'O' : 'X';
        updateStatus(currentPlayer === 'X' ? `⭐ دور ${playerName} (X)` : `🤖 دور الذكاء (O)`);
        return true;
    }
    
    // ---------- الذكاء الاصطناعي المتقدم ----------
    function getEmptyIndices(board) {
        return board.reduce((arr, cell, idx) => {
            if(cell === '') arr.push(idx);
            return arr;
        }, []);
    }
    
    // خوارزمية Minimax البسيطة للمحترف
    function minimax(newBoard, depth, isMaximizing, aiSym, humanSym) {
        let winnerCheck = null;
        if(checkWinner(newBoard, aiSym)) return 10 - depth;
        if(checkWinner(newBoard, humanSym)) return depth - 10;
        if(isDraw(newBoard)) return 0;
        
        if(isMaximizing) {
            let best = -Infinity;
            for(let i=0; i<9; i++) {
                if(newBoard[i] === '') {
                    newBoard[i] = aiSym;
                    let score = minimax(newBoard, depth+1, false, aiSym, humanSym);
                    newBoard[i] = '';
                    best = Math.max(score, best);
                }
            }
            return best;
        } else {
            let best = Infinity;
            for(let i=0; i<9; i++) {
                if(newBoard[i] === '') {
                    newBoard[i] = humanSym;
                    let score = minimax(newBoard, depth+1, true, aiSym, humanSym);
                    newBoard[i] = '';
                    best = Math.min(score, best);
                }
            }
            return best;
        }
    }
    
    function getBestMove(board, aiSym, humanSym) {
        let bestScore = -Infinity;
        let bestMove = -1;
        for(let i=0; i<9; i++) {
            if(board[i] === '') {
                board[i] = aiSym;
                let score = minimax(board, 0, false, aiSym, humanSym);
                board[i] = '';
                if(score > bestScore) {
                    bestScore = score;
                    bestMove = i;
                }
            }
        }
        return bestMove;
    }
    
    // حركة الذكاء حسب المستوى
    function aiMove() {
        if(!gameActive) return;
        if(currentPlayer !== aiSymbol) return;
        
        let empty = getEmptyIndices(boardState);
        if(empty.length === 0) return;
        
        let moveIndex = -1;
        // ضعيف: عشوائي
        if(aiLevel === 'weak') {
            let rand = Math.floor(Math.random() * empty.length);
            moveIndex = empty[rand];
        }
        // طبيعي : عشوائي أحياناً + منع خسارة فورية اذا وجد
        else if(aiLevel === 'normal') {
            // منع فوز الخصم
            for(let i of empty) {
                boardState[i] = humanSymbol;
                if(checkWinner(boardState, humanSymbol)) {
                    moveIndex = i;
                    boardState[i] = '';
                    break;
                }
                boardState[i] = '';
            }
            if(moveIndex === -1) {
                // عشوائي
                let rand = Math.floor(Math.random() * empty.length);
                moveIndex = empty[rand];
            }
        }
        // ذكي : يحاول الفوز + منع الخصم
        else if(aiLevel === 'smart') {
            // فوز مباشر
            for(let i of empty) {
                boardState[i] = aiSymbol;
                if(checkWinner(boardState, aiSymbol)) {
                    moveIndex = i;
                    boardState[i] = '';
                    break;
                }
                boardState[i] = '';
            }
            if(moveIndex === -1) {
                for(let i of empty) {
                    boardState[i] = humanSymbol;
                    if(checkWinner(boardState, humanSymbol)) {
                        moveIndex = i;
                        boardState[i] = '';
                        break;
                    }
                    boardState[i] = '';
                }
            }
            if(moveIndex === -1) {
                // أفضل مركز متوسط
                const center = 4, corners = [0,2,6,8];
                if(empty.includes(center)) moveIndex = center;
                else {
                    let availCorners = corners.filter(c => empty.includes(c));
                    if(availCorners.length) moveIndex = availCorners[Math.floor(Math.random()*availCorners.length)];
                    else moveIndex = empty[0];
                }
            }
        }
        // محترف (minimax كامل)
        else if(aiLevel === 'pro') {
            moveIndex = getBestMove([...boardState], aiSymbol, humanSymbol);
        }
        
        if(moveIndex !== -1) {
            setTimeout(() => {
                if(gameActive && currentPlayer === aiSymbol) {
                    makeMove(moveIndex, aiSymbol);
                    // بعد الحركة، اذا اللعبة ما زالت نشطة والدور انتقل إلى الإنسان
                }
            }, 100);
        }
    }
    
    // استدعاء الذكاء بعد تأخير بسيط
    function triggerAI() {
        if(!gameActive) return;
        if(currentPlayer === aiSymbol) {
            aiMove();
        }
    }
    
    // انشاء اللوحة
    function createBoardUI() {
        boardDiv.innerHTML = '';
        for(let i=0; i<9; i++) {
            const cell = document.createElement('div');
            cell.classList.add('cell');
            cell.setAttribute('data-idx', i);
            cell.addEventListener('click', (function(idx) {
                return function() {
                    if(!gameActive) return;
                    if(currentPlayer === humanSymbol && boardState[idx] === '') {
                        makeMove(idx, humanSymbol);
                        // بعد الحركة تحقق انتهاء ثم استدعاء الذكاء
                        setTimeout(() => {
                            if(gameActive && currentPlayer === aiSymbol) {
                                triggerAI();
                            }
                        }, 150);
                    }
                };
            })(i));
            boardDiv.appendChild(cell);
        }
        renderBoard();
    }
    
    // إعادة ضبط اللعبة بالكامل
    function resetGameBoard() {
        boardState = ['', '', '', '', '', '', '', '', ''];
        currentPlayer = 'X';   // الإنسان يبدأ دائما
        gameActive = true;
        renderBoard();
        updateStatus(`⭐ دور ${playerName} (X)`);
        // إذا كان AI يبدأ لأي سبب؟ لا, X للإنسان دائما.
        // لكن إن أردنا بدل؟ لا X إنسان.
    }
    
    // بدء الجلسة الكاملة
    function startMatch() {
        playerName = playerNameInput.value.trim();
        if(playerName === "") playerName = "بطل";
        aiLevel = aiLevelSelect.value;
        // إعادة تهيئة
        boardState = ['', '', '', '', '', '', '', '', ''];
        currentPlayer = 'X';
        gameActive = true;
        humanSymbol = 'X';
        aiSymbol = 'O';
        renderBoard();
        updateStatus(`🎮 ${playerName} (X) ضد الذكاء (O) | المستوى: ${aiLevelSelect.options[aiLevelSelect.selectedIndex].text}`);
        // بدأ اللعب بدون حركة AI لأنه دور X الإنسان
        // لكن ننشط audio context عند أول تفاعل
        initAudio();
        // إذا كان أي شيء آخر ... ممتاز
    }
    
    // اظهار الشاشات
    function showGame() {
        startScreen.style.display = 'none';
        gameScreen.style.display = 'flex';
        createBoardUI();
        startMatch();
    }
    
    function showStartMenu() {
        startScreen.style.display = 'block';
        gameScreen.style.display = 'none';
        // إيقاف الصوتيات لا حاجة
    }
    
    // إعادة اللعبة دون الخروج للقائمة
    function resetAndRestart() {
        resetGameBoard();
        gameActive = true;
        currentPlayer = 'X';
        // مسح اللوحة تماما
        boardState = ['', '', '', '', '', '', '', '', ''];
        renderBoard();
        updateStatus(`⭐ دور ${playerName} (X)`);
        // إذا كان AI لا يتحرك لأنه X يبدأ.
    }
    
    // نطق النص باستخدام SpeechSynthesis بنبرة حادة
    function speakText(text) {
        if(!window.speechSynthesis) return;
        const utterance = new SpeechSynthesisUtterance(text);
        utterance.lang = 'ar-SA';
        utterance.rate = 1.0;
        utterance.pitch = 1.3;   // نبرة حادة وجميلة
        utterance.volume = 0.9;
        window.speechSynthesis.cancel();
        window.speechSynthesis.speak(utterance);
    }
    
    // إضافة مستمعي الأحداث
    beginBtn.addEventListener('click', () => {
        initAudio();
        showGame();
    });
    resetBtn.addEventListener('click', () => {
        resetAndRestart();
        playDropSound(); // تأكيد الصوت
    });
    backMenuBtn.addEventListener('click', () => {
        showStartMenu();
        if(window.speechSynthesis) window.speechSynthesis.cancel();
    });
    
    // تحضير أولي
    createBoardUI();
    startScreen.style.display = 'block';
    gameScreen.style.display = 'none';
</script>
</body>
</html>
# bi12
X/O game 
