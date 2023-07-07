<template>
  <div class="tic-tac-toe-widget">
    <h2 class="widget-title">Widget TicTacToe</h2>
    <div class="board">
      <div v-for="(cell, index) in board" :key="index" @click="handleClick(index)">{{ cell }}</div>
    </div>
    <button @click="resetGame" class="reset-button">Reset</button>
    <p>{{ status }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: ['', '', '', '', '', '', '', '', ''],
      currentPlayer: 'X',
      isGameEnded: false,
    };
  },
  computed: {
    status() {
      if (this.isGameEnded) {
        return 'Game Over!';
      }
      return `Player ${this.currentPlayer}'s turn`;
    },
  },
  methods: {
    handleClick(index) {
      if (!this.isGameEnded && this.board[index] === '') {
        this.board[index] = this.currentPlayer;
        this.checkWinner();
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X';
      }
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
        [0, 4, 8], [2, 4, 6], // Diagonals
      ];

      for (const combination of winningCombinations) {
        const [a, b, c] = combination;
        if (this.board[a] && this.board[a] === this.board[b] && this.board[a] === this.board[c]) {
          this.isGameEnded = true;
          return;
        }
      }
    },
    resetGame() {
      this.board = ['', '', '', '', '', '', '', '', ''];
      this.currentPlayer = 'X';
      this.isGameEnded = false;
    },
  },
};
</script>

<style scoped>
.tic-tac-toe-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  text-align: center;
  background-color: #ffffff;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.widget-title {
  margin-top: 0;
  color: #333;
  font-size: 24px;
}
.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  margin-bottom: 10px;
}

.board div {
  border: 1px solid #ccc;
  height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.reset-button {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #084d71;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
  transition: background-color 0.3s ease;
}

.reset-button:hover {
  background-color: #0056b3;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
