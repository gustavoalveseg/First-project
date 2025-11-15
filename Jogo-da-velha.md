# First-project
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Velha Unificado</title>

    <style>
        body {
            font-family: 'Arial', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background-color: #282c34;
            color: white;
        }

        .container {
            text-align: center;
            padding: 20px;
            border-radius: 10px;
            background-color: #3e4451;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.5);
        }

        h1 {
            margin-bottom: 20px;
            color: #61afef;
        }

        #status {
            margin-bottom: 15px;
            font-size: 1.3em;
            font-weight: bold;
        }

        .tabuleiro {
            display: grid;
            grid-template-columns: repeat(3, 100px);
            grid-gap: 5px;
            margin: 0 auto;
        }

        .cell {
            width: 100px;
            height: 100px;
            background-color: #4b5263;
            border: 2px solid #282c34;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 3.5em;
            cursor: pointer;
            user-select: none;
            transition: background-color 0.2s, transform 0.1s;
        }

        .cell:hover {
            background-color: #61afef;
        }

        .cell.winner {
            background-color: #e06c75;
            color: #ffffff;
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }

        #restartButton {
            margin-top: 25px;
            padding: 12px 25px;
            font-size: 1.1em;
            cursor: pointer;
            background-color: #c678dd;
            color: white;
            border: none;
            border-radius: 8px;
            transition: background-color 0.2s, transform 0.1s;
            box-shadow: 0 4px #9e5bd0;
        }

        #restartButton:active {
            box-shadow: 0 2px #9e5bd0;
            transform: translateY(2px);
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Jogo da Velha</h1>
        <div id="status"></div>
        <div id="gameBoard" class="tabuleiro">
            </div>
        <button id="restartButton">Reiniciar Jogo</button>
    </div>

    <script>
        // Seleciona elementos do DOM
        const gameBoard = document.getElementById('gameBoard');
        const statusDisplay = document.getElementById('status');
        const restartButton = document.getElementById('restartButton');

        // Estado inicial do jogo
        let board = ['', '', '', '', '', '', '', '', ''];
        let currentPlayer = 'X';
        let gameActive = true; 

        // Combinações de vitória
        const winningCombinations = [
            [0, 1, 2], [3, 4, 5], [6, 7, 8],
            [0, 3, 6], [1, 4, 7], [2, 5, 8],
            [0, 4, 8], [2, 4, 6]
        ];

        // --- Funções Auxiliares ---

        function updateStatus(message) {
            statusDisplay.textContent = message;
        }

        // --- Lógica Principal ---

        function checkResult() {
            let roundWon = false;

            for (let i = 0; i < winningCombinations.length; i++) {
                const combo = winningCombinations[i];
                let a = board[combo[0]];
                let b = board[combo[1]];
                let c = board[combo[2]];

                if (a !== '' && a === b && b === c) {
                    roundWon = true;
                    gameActive = false;
                    
                    // Adiciona classe 'winner' para estilizar as células
                    const cells = gameBoard.children;
                    cells[combo[0]].classList.add('winner');
                    cells[combo[1]].classList.add('winner');
                    cells[combo[2]].classList.add('winner');
                    
                    updateStatus(`Jogador ${currentPlayer} VENCEU! 🎉`);
                    break;
                }
            }

            // Verifica empate (se o tabuleiro está cheio e ninguém venceu)
            if (!roundWon && !board.includes('')) {
                gameActive = false;
                updateStatus("Empate! 🤝");
            }
        }

        function handleCellClick(event) {
            // Sai se o jogo não estiver ativo ou a célula já estiver preenchida
            if (!gameActive || event.target.textContent !== '') {
                return;
            }

            const clickedCell = event.target;
            const clickedCellIndex = parseInt(clickedCell.getAttribute('data-index'));

            board[clickedCellIndex] = currentPlayer;
            clickedCell.textContent = currentPlayer;

            checkResult();

            if (gameActive) {
                currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
                updateStatus(`Vez do Jogador ${currentPlayer}`);
            }
        }

        function createBoard() {
            // Limpa o tabuleiro e recria as 9 células
            gameBoard.innerHTML = ''; 
            for (let i = 0; i < 9; i++) {
                const cell = document.createElement('div');
                cell.classList.add('cell');
                cell.setAttribute('data-index', i);
                cell.addEventListener('click', handleCellClick);
                gameBoard.appendChild(cell);
            }
            updateStatus(`Vez do Jogador ${currentPlayer}`);
        }

        function restartGame() {
            board = ['', '', '', '', '', '', '', '', ''];
            currentPlayer = 'X';
            gameActive = true;
            gameBoard.innerHTML = '';
            
            createBoard();
        }

        // --- Inicialização ---

        restartButton.addEventListener('click', restartGame);
        
        // Inicia o tabuleiro quando a página carrega
        createBoard();
    </script>
</body>
</html>
