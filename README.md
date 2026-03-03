[online_viewer_net (2).htm](https://github.com/user-attachments/files/25725211/online_viewer_net.2.htm)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Connect 4 - Two Player Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            padding: 20px;
        }

        .game-container {
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 60px rgba(0,0,0,0.3);
            max-width: 800px;
            width: 100%;
        }

        h1 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
            font-size: 2.5em;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .game-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
        }

        .player-turn {
            font-size: 1.2em;
            font-weight: bold;
            padding: 10px 20px;
            border-radius: 5px;
            transition: all 0.3s ease;
        }

        .player-red {
            background: #ff4757;
            color: white;
        }

        .player-yellow {
            background: #ffa502;
            color: white;
        }

        .game-status {
            font-size: 1.1em;
            color: #2f3542;
        }

        .reset-btn {
            background: #70a1ff;
            color: white;
            border: none;
            padding: 10px 25px;
            border-radius: 5px;
            font-size: 1em;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid transparent;
        }

        .reset-btn:hover {
            background: #1e90ff;
            transform: scale(1.05);
        }

        .reset-btn:active {
            transform: scale(0.95);
        }

        .board {
            display: flex;
            flex-direction: column;
            background: #2f3542;
            padding: 20px;
            border-radius: 15px;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.5);
        }

        .row {
            display: flex;
            justify-content: center;
        }

        .cell {
            width: 80px;
            height: 80px;
            background: white;
            border-radius: 50%;
            margin: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            position: relative;
            overflow: hidden;
        }

        .cell::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, rgba(255,255,255,0.8), transparent);
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .cell:hover::before {
            opacity: 1;
        }

        .cell.red {
            background: #ff4757;
            animation: dropPiece 0.5s ease-out;
        }

        .cell.yellow {
            background: #ffa502;
            animation: dropPiece 0.5s ease-out;
        }

        .cell.winner {
            animation: winner 1s ease-in-out infinite;
            box-shadow: 0 0 20px gold;
        }

        @keyframes dropPiece {
            0% {
                transform: translateY(-300px) rotate(180deg);
                opacity: 0;
            }
            70% {
                transform: translateY(10px) rotate(0deg);
            }
            85% {
                transform: translateY(-5px);
            }
            100% {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
        }

        @keyframes winner {
            0%, 100% {
                transform: scale(1);
                box-shadow: 0 0 20px gold;
            }
            50% {
                transform: scale(1.1);
                box-shadow: 0 0 40px gold;
            }
        }

        .stats-panel {
            margin-top: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
            display: flex;
            justify-content: space-around;
        }

        .stat-item {
            text-align: center;
        }

        .stat-label {
            font-size: 0.9em;
            color: #747d8c;
            margin-bottom: 5px;
        }

        .stat-value {
            font-size: 1.3em;
            font-weight: bold;
            color: #2f3542;
        }

        .move-history {
            margin-top: 20px;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
            max-height: 150px;
            overflow-y: auto;
        }

        .history-title {
            font-weight: bold;
            margin-bottom: 10px;
            color: #2f3542;
        }

        .history-entry {
            padding: 5px 10px;
            margin: 3px 0;
            background: white;
            border-radius: 3px;
            font-size: 0.9em;
            color: #2f3542;
            border-left: 3px solid;
            animation: slideIn 0.3s ease-out;
        }

        @keyframes slideIn {
            from {
                transform: translateX(-20px);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .history-entry.red {
            border-left-color: #ff4757;
        }

        .history-entry.yellow {
            border-left-color: #ffa502;
        }

        .footer {
            margin-top: 20px;
            text-align: center;
            color: #747d8c;
            font-size: 0.9em;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>🎮 CONNECT 4 🎮</h1>
        
        <div class="game-info">
            <div class="player-turn" id="turnIndicator">Red's Turn</div>
            <div class="game-status" id="gameStatus">Drop a piece to start!</div>
            <button class="reset-btn" id="resetGame">🔄 New Game</button>
        </div>

        <div class="board" id="board"></div>

        <div class="stats-panel">
            <div class="stat-item">
                <div class="stat-label">Red Wins</div>
                <div class="stat-value" id="redWins">0</div>
            </div>
            <div class="stat-item">
                <div class="stat-label">Yellow Wins</div>
                <div class="stat-value" id="yellowWins">0</div>
            </div>
            <div class="stat-item">
                <div class="stat-label">Draws</div>
                <div class="stat-value" id="draws">0</div>
            </div>
        </div>

        <div class="move-history" id="moveHistory">
            <div class="history-title">📜 Move History</div>
        </div>

        <div class="footer">
            Click any column to drop a piece • Two Player Game
        </div>
    </div>

    <script>
        // Game state object (Model)
        const gameState = {
            board: [],
            currentPlayer: 'red',
            gameOver: false,
            winner: null,
            moveCount: 0,
            stats: {
                redWins: 0,
                yellowWins: 0,
                draws: 0
            },
            moveHistory: []
        };

        // Constants
        const ROWS = 6;
        const COLS = 7;

        // DOM elements
        const boardElement = document.getElementById('board');
        const turnIndicator = document.getElementById('turnIndicator');
        const gameStatus = document.getElementById('gameStatus');
        const resetButton = document.getElementById('resetGame');
        const redWinsElement = document.getElementById('redWins');
        const yellowWinsElement = document.getElementById('yellowWins');
        const drawsElement = document.getElementById('draws');
        const moveHistoryElement = document.getElementById('moveHistory');

        // Initialize the game
        function initGame() {
            gameState.board = Array(ROWS).fill().map(() => Array(COLS).fill(null));
            gameState.currentPlayer = 'red';
            gameState.gameOver = false;
            gameState.winner = null;
            gameState.moveCount = 0;
            gameState.moveHistory = [];
            updateTurnIndicator();
            gameStatus.textContent = 'Drop a piece to start!';
            renderBoard();
            updateMoveHistory();
        }

        // Render the board based on game state
        function renderBoard() {
            // Clear the board
            boardElement.innerHTML = '';

            // Create rows and cells
            for (let row = 0; row < ROWS; row++) {
                const rowElement = document.createElement('div');
                rowElement.className = 'row';
                
                for (let col = 0; col < COLS; col++) {
                    const cell = document.createElement('div');
                    cell.className = 'cell';
                    cell.dataset.row = row;
                    cell.dataset.col = col;
                    
                    // Add piece if exists
                    if (gameState.board[row][col]) {
                        cell.classList.add(gameState.board[row][col]);
                    }
                    
                    // Add click event
                    cell.addEventListener('click', handleCellClick);
                    
                    // Add mouseover event for visual feedback
                    cell.addEventListener('mouseover', handleCellHover);
                    cell.addEventListener('mouseout', handleCellHoverOut);
                    
                    rowElement.appendChild(cell);
                }
                
                boardElement.appendChild(rowElement);
            }
        }

        // Handle cell click
        function handleCellClick(event) {
            if (gameState.gameOver) {
                gameStatus.textContent = 'Game is over! Click "New Game" to play again.';
                return;
            }

            const col = parseInt(event.target.dataset.col);
            dropPiece(col);
        }

        // Handle cell hover
        function handleCellHover(event) {
            if (gameState.gameOver) return;
            
            const col = event.target.dataset.col;
            const row = getLowestEmptyRow(parseInt(col));
            
            if (row !== -1) {
                // Add hover effect by temporarily changing styling
                const cellsInColumn = document.querySelectorAll(`[data-col="${col}"]`);
                cellsInColumn.forEach(cell => {
                    cell.style.transform = 'scale(1.05)';
                    cell.style.boxShadow = '0 0 15px rgba(255,255,255,0.5)';
                });
            }
        }

        function handleCellHoverOut(event) {
            const col = event.target.dataset.col;
            const cellsInColumn = document.querySelectorAll(`[data-col="${col}"]`);
            cellsInColumn.forEach(cell => {
                cell.style.transform = 'scale(1)';
                cell.style.boxShadow = '';
            });
        }

        // Find the lowest empty row in a column
        function getLowestEmptyRow(col) {
            for (let row = ROWS - 1; row >= 0; row--) {
                if (!gameState.board[row][col]) {
                    return row;
                }
            }
            return -1; // Column is full
        }

        // Drop a piece in the specified column
        function dropPiece(col) {
            const row = getLowestEmptyRow(col);
            
            if (row === -1) {
                gameStatus.textContent = 'Column is full! Choose another column.';
                return; // Column is full
            }

            // Update game state
            gameState.board[row][col] = gameState.currentPlayer;
            gameState.moveCount++;
            
            // Add to move history
            addToMoveHistory(row, col);
            
            // Update DOM with animation
            updateBoardCell(row, col, gameState.currentPlayer);
            
            // Check for win or draw
            if (checkWin(row, col, gameState.currentPlayer)) {
                gameState.gameOver = true;
                gameState.winner = gameState.currentPlayer;
                gameState.stats[`${gameState.currentPlayer}Wins`]++;
                gameStatus.textContent = `${gameState.currentPlayer.toUpperCase()} wins! 🎉`;
                highlightWinningPieces(row, col, gameState.currentPlayer);
                updateStats();
            } else if (gameState.moveCount === ROWS * COLS) {
                gameState.gameOver = true;
                gameState.stats.draws++;
                gameStatus.textContent = "It's a draw! 🤝";
                updateStats();
            } else {
                // Switch players
                gameState.currentPlayer = gameState.currentPlayer === 'red' ? 'yellow' : 'red';
                updateTurnIndicator();
                gameStatus.textContent = `${gameState.currentPlayer}'s turn`;
            }
        }

        // Update a single cell in the DOM with animation
        function updateBoardCell(row, col, player) {
            const cells = document.querySelectorAll(`[data-row="${row}"][data-col="${col}"]`);
            cells.forEach(cell => {
                cell.classList.add(player);
            });
        }

        // Add move to history and update DOM
        function addToMoveHistory(row, col) {
            const moveNumber = gameState.moveHistory.length + 1;
            const player = gameState.currentPlayer;
            const moveText = `Move ${moveNumber}: ${player.toUpperCase()} played at column ${col + 1}`;
            
            gameState.moveHistory.push({
                player: player,
                text: moveText,
                row: row,
                col: col
            });
            
            updateMoveHistory();
        }

        // Update move history display
        function updateMoveHistory() {
            // Remove old history entries
            const oldEntries = moveHistoryElement.querySelectorAll('.history-entry');
            oldEntries.forEach(entry => entry.remove());
            
            // Add new entries
            gameState.moveHistory.forEach((move, index) => {
                const entry = document.createElement('div');
                entry.className = `history-entry ${move.player}`;
                entry.textContent = move.text;
                moveHistoryElement.appendChild(entry);
                
                // Scroll to bottom
                moveHistoryElement.scrollTop = moveHistoryElement.scrollHeight;
            });
        }

        // Check for win condition
        function checkWin(row, col, player) {
            // Check horizontal
            for (let c = 0; c <= COLS - 4; c++) {
                if (gameState.board[row][c] === player &&
                    gameState.board[row][c + 1] === player &&
                    gameState.board[row][c + 2] === player &&
                    gameState.board[row][c + 3] === player) {
                    return true;
                }
            }

            // Check vertical
            for (let r = 0; r <= ROWS - 4; r++) {
                if (gameState.board[r][col] === player &&
                    gameState.board[r + 1][col] === player &&
                    gameState.board[r + 2][col] === player &&
                    gameState.board[r + 3][col] === player) {
                    return true;
                }
            }

            // Check diagonal (down-right)
            for (let r = 0; r <= ROWS - 4; r++) {
                for (let c = 0; c <= COLS - 4; c++) {
                    if (gameState.board[r][c] === player &&
                        gameState.board[r + 1][c + 1] === player &&
                        gameState.board[r + 2][c + 2] === player &&
                        gameState.board[r + 3][c + 3] === player) {
                        return true;
                    }
                }
            }

            // Check diagonal (up-right)
            for (let r = 3; r < ROWS; r++) {
                for (let c = 0; c <= COLS - 4; c++) {
                    if (gameState.board[r][c] === player &&
                        gameState.board[r - 1][c + 1] === player &&
                        gameState.board[r - 2][c + 2] === player &&
                        gameState.board[r - 3][c + 3] === player) {
                        return true;
                    }
                }
            }

            return false;
        }

        // Highlight winning pieces
        function highlightWinningPieces(row, col, player) {
            // This is a simplified version - in a real implementation,
            // you would find and highlight all winning pieces
            const cells = document.querySelectorAll('.cell');
            cells.forEach(cell => {
                if (cell.classList.contains(player)) {
                    cell.classList.add('winner');
                }
            });
        }

        // Update turn indicator styling
        function updateTurnIndicator() {
            turnIndicator.textContent = `${gameState.currentPlayer === 'red' ? 'Red' : 'Yellow'}'s Turn`;
            turnIndicator.className = `player-turn player-${gameState.currentPlayer}`;
        }

        // Update statistics display
        function updateStats() {
            redWinsElement.textContent = gameState.stats.redWins;
            yellowWinsElement.textContent = gameState.stats.yellowWins;
            drawsElement.textContent = gameState.stats.draws;
        }

        // Reset game
        function resetGame() {
            initGame();
        }

        // Load stats from localStorage
        function loadStats() {
            const savedStats = localStorage.getItem('connect4_stats');
            if (savedStats) {
                gameState.stats = JSON.parse(savedStats);
                updateStats();
            }
        }

        // Save stats to localStorage
        function saveStats() {
            localStorage.setItem('connect4_stats', JSON.stringify(gameState.stats));
        }

        // Event listeners
        resetButton.addEventListener('click', () => {
            saveStats(); // Save stats before reset
            resetGame();
        });

        // Save stats when window is closed
        window.addEventListener('beforeunload', () => {
            saveStats();
        });

        // Initialize the game when page loads
        window.addEventListener('load', () => {
            initGame();
            loadStats();
            
            // Add keyboard event for bonus interactivity
            document.addEventListener('keydown', (e) => {
                if (e.key === 'r' || e.key === 'R') {
                    resetGame();
                }
            });
        });

        // Touch events for mobile
        boardElement.addEventListener('touchstart', (e) => {
            e.preventDefault();
            const cell = e.target.closest('.cell');
            if (cell) {
                const col = cell.dataset.col;
                if (col !== undefined) {
                    dropPiece(parseInt(col));
                }
            }
        });
    </script>
</body>
</html>
