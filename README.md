# Tri-game 
<html>
<head>
  <titel>XOX Game</titel>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }
    h1 {
      margin-bottom: 20px;
      color: #333;
    }
    .board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-template-rows: repeat(3, 100px);
      gap: 5px;
    }
    .cell {
      background: white;
      border: 2px solid #444;
      font-size: 2.5rem;
      font-weight: bold;
      text-align: center;
      line-height: 100px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .cell:hover {
      background: #ddd;
    }
    #status {
      margin-top: 20px;
      font-size: 1.2rem;
      color: #333;
    }
    button {
      margin-top: 15px;
      padding: 8px 16px;
      font-size: 1rem;
      border: none;
      background: #4CAF50;
      color: white;
      cursor: pointer;
      border-radius: 4px;
    }
    button:hover {
      background: #45a049;
    }
  </style>
</head>
<body>
  <h1>XOX Game</h1>
  <div class="board" id="board">
    <div class="cell" data-index="0"></div>
    <div class="cell" data-index="1"></div>
    <div class="cell" data-index="2"></div>
    <div class="cell" data-index="3"></div>
    <div class="cell" data-index="4"></div>
    <div class="cell" data-index="5"></div>
    <div class="cell" data-index="6"></div>
    <div class="cell" data-index="7"></div>
    <div class="cell" data-index="8"></div>
  </div>
  <p id="status">Player X's turn</p>
  <button id="resetBtn">Restart</button>

  <script>
    const cells = document.querySelectorAll('.cell');
    const statusText = document.getElementById('status');
    const resetBtn = document.getElementById('resetBtn');

    let currentPlayer = 'X';
    let gameActive = true;
    let gameState = ["", "", "", "", "", "", "", "", ""];

    const winningPatterns = [
      [0,1,2], [3,4,5], [6,7,8], // Rows
      [0,3,6], [1,4,7], [2,5,8], // Columns
      [0,4,8], [2,4,6]           // Diagonals
    ];

    function handleCellClick(e) {
      const index = e.target.dataset.index;
      if (gameState[index] !== "" || !gameActive) return;
      gameState[index] = currentPlayer;
      e.target.textContent = currentPlayer;
      checkWinner();
    }

    function checkWinner() {
      let winnerFound = false;

      for (let pattern of winningPatterns) {
        const [a,b,c] = pattern;
        if (gameState[a] && gameState[a] === gameState[b] && gameState[a] === gameState[c]) {
          winnerFound = true;
          break;
        }
      }

      if (winnerFound) {
        statusText.textContent = `🎉 Player ${currentPlayer} Wins!`;
        gameActive = false;
        return;
      }

      if (!gameState.includes("")) {
        statusText.textContent = "😃 It's a Draw!";
        gameActive = false;
        return;
      }

      currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      statusText.textContent = `Player ${currentPlayer}'s turn`;
    }

    function resetGame() {
      currentPlayer = 'X';
      gameActive = true;
      gameState = ["", "", "", "", "", "", "", "", ""];
      cells.forEach(cell => cell.textContent = "");
      statusText.textContent = "Player X's turn";
    }

    cells.forEach(cell => cell.addEventListener('click', handleCellClick));
    resetBtn.addEventListener('click', resetGame);
  </script>
</body>
</html>
