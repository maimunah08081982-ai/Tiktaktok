<!DOCTYPE html><html lang="id">
<head>
  <meta charset="UTF-8" />
  <title>Tic Tac Toe Animasi</title>
  <style>
    body {
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background: linear-gradient(135deg, #1d2671, #c33764);
      font-family: Arial, sans-serif;
      color: white;
    }.game {
  background: rgba(0,0,0,0.35);
  padding: 25px 30px 30px;
  border-radius: 20px;
  box-shadow: 0 20px 40px rgba(0,0,0,0.4);
  animation: float 3s ease-in-out infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

h1 {
  text-align: center;
  margin-bottom: 10px;
  letter-spacing: 2px;
}

.status {
  text-align: center;
  margin-bottom: 15px;
  font-size: 18px;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-template-rows: repeat(3, 100px);
  gap: 10px;
}

.cell {
  background: rgba(255,255,255,0.15);
  border-radius: 15px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 48px;
  cursor: pointer;
  transition: transform 0.2s, background 0.3s;
}

.cell:hover {
  background: rgba(255,255,255,0.25);
  transform: scale(1.05);
}

.cell.taken {
  cursor: not-allowed;
}

.cell.x {
  color: #00eaff;
  animation: pop 0.3s ease;
}

.cell.o {
  color: #ffdf6c;
  animation: pop 0.3s ease;
}

@keyframes pop {
  0% { transform: scale(0); }
  100% { transform: scale(1); }
}

button {
  margin-top: 20px;
  width: 100%;
  padding: 10px;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  cursor: pointer;
  background: linear-gradient(135deg, #00eaff, #ffdf6c);
  color: #333;
  font-weight: bold;
  transition: transform 0.2s;
}

button:hover {
  transform: scale(1.05);
}

  </style>
</head>
<body>
  <div class="game">
    <h1>TIC TAC TOE</h1>
    <div class="status" id="status">Giliran: X</div>
    <div class="board" id="board"></div>
    <button onclick="resetGame()">Ulangi Game</button>
  </div>  <script>
    const board = document.getElementById('board');
    const statusText = document.getElementById('status');

    let currentPlayer = 'X';
    let gameActive = true;
    let gameState = ['', '', '', '', '', '', '', '', ''];

    const winConditions = [
      [0,1,2], [3,4,5], [6,7,8],
      [0,3,6], [1,4,7], [2,5,8],
      [0,4,8], [2,4,6]
    ];

    function createBoard() {
      board.innerHTML = '';
      gameState.forEach((cell, index) => {
        const cellDiv = document.createElement('div');
        cellDiv.classList.add('cell');
        cellDiv.dataset.index = index;
        cellDiv.addEventListener('click', handleCellClick);
        board.appendChild(cellDiv);
      });
    }

    function handleCellClick(e) {
      const index = e.target.dataset.index;

      if (gameState[index] !== '' || !gameActive) return;

      gameState[index] = currentPlayer;
      e.target.textContent = currentPlayer;
      e.target.classList.add('taken', currentPlayer.toLowerCase());

      checkResult();
    }

    function checkResult() {
      let roundWon = false;

      for (let condition of winConditions) {
        const [a, b, c] = condition;
        if (gameState[a] && gameState[a] === gameState[b] && gameState[a] === gameState[c]) {
          roundWon = true;
          break;
        }
      }

      if (roundWon) {
        statusText.textContent = `Pemain ${currentPlayer} Menang!`;
        gameActive = false;
        return;
      }

      if (!gameState.includes('')) {
        statusText.textContent = 'Hasil Seri!';
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      statusText.textContent = `Giliran: ${currentPlayer}`;
    }

    function resetGame() {
      currentPlayer = 'X';
      gameActive = true;
      gameState = ['', '', '', '', '', '', '', '', ''];
      statusText.textContent = 'Giliran: X';
      createBoard();
    }

    createBoard();
  </script></body>
</html>
