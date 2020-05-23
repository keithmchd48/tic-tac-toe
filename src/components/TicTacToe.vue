<template>
  <div>
    <div class="board" :class="X_OR_CIRCLE" id="board">
      <div v-for="(cell, index) in 9" :key="index"
           @click.once="handleClick($event, index)"
           class="cell" data-cell>
      </div>
    </div>
    <div class="winning-message" :class="{'show': show}" id="winningMessage">
      <div>{{messageWins}}</div>
      <button id="restartButton" @click="$emit('forceRender')">Restart</button>
    </div>
  </div>
</template>

<script>
  const X_CLASS = 'x'
  const CIRCLE_CLASS = 'circle'
  const WINNING_COMBINATIONS = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ]
  export default {
    name: 'TicTacToe',
    data () {
      return {
        circleTurn: false,
        show: false,
        messageWins: '',
        game: [null, null, null, null, null, null, null, null, null]
      }
    },
    computed: {
      X_OR_CIRCLE () {
        if (this.circleTurn) return CIRCLE_CLASS
        else return X_CLASS
      }
    },
    methods: {
      handleClick (event, cellNumber) {
        const cell = event.target
        // add class to clicked cell
        cell.classList.add(this.X_OR_CIRCLE)
        this.game[cellNumber] = this.X_OR_CIRCLE
        // check for win
        if (this.checkWin(this.X_OR_CIRCLE)) {
          this.messageWins = this.circleTurn ? `O's win!` : `X's win!`
          this.show = true
          return false
        }
        // check for draw
        if (this.isDraw()) {
          this.messageWins = `It's a draw!`
          this.show = true
          return false
        }
        // switch turns
        this.circleTurn = !this.circleTurn
      },
      checkWin (currentClass) {
        return WINNING_COMBINATIONS.some(combination => {
          const [a, b, c] = combination
          return this.game[a] === currentClass && this.game[b] === currentClass && this.game[c] === currentClass
        })
      },
      isDraw () {
        return this.game.every(square => {
          return Boolean(square) === true
        })
      }
    }
  }
</script>

<style>
  :root {
    --cell-size: 100px;
    --mark-size: calc(var(--cell-size)*0.9)
  }

  .board {
    width: 100vw;
    height: 100vh;
    display: grid;
    grid-template-columns: repeat(3, auto);
    justify-content: center;
    align-content: center;
    justify-items: center;
    align-items: center;
  }

  .cell {
    width: var(--cell-size);
    height: var(--cell-size);
    border: 1px solid black;
    display: flex;
    justify-content: center;
    align-content: center;
    align-items: center;
    position: relative;
    cursor: pointer;
  }
  .cell.x,
  .cell.circle {
    cursor: not-allowed;
  }

  .cell:nth-child(1),
  .cell:nth-child(2),
  .cell:nth-child(3) {
    border-top: none;
  }
  .cell:nth-child(7),
  .cell:nth-child(8),
  .cell:nth-child(9) {
    border-bottom: none;
  }
  /* cell nos. 1, 4, 7 */
  .cell:nth-child(3n + 1) {
    border-left: none;
  }
  /* cell nos. 3,6,9 */
  .cell:nth-child(3n) {
    border-right: none;
  }


  .board.x .cell:not(.x):not(.circle):hover {
    opacity: 0.3;
  }
  .board.circle .cell:not(.x):not(.circle):hover {
    opacity: 0.3;
  }

  /* X design*/
  .cell.x::before,
  .cell.x::after,
  .board.x .cell:not(.x):not(.circle):hover::before,
  .board.x .cell:not(.x):not(.circle):hover::after {
    content: '';
    position: absolute;
    width: calc(var(--mark-size) * 0.15);
    height: var(--mark-size);
    background-color: black;
  }
  .cell.x::before,
  .board.x .cell:not(.x):not(.circle):hover::before {
    transform: rotate(45deg);
  }
  .cell.x::after,
  .board.x .cell:not(.x):not(.circle):hover::after {
    transform: rotate(-45deg);
  }

  /*Circle design*/
  .cell.circle::before,
  .cell.circle::after,
  .board.circle .cell:not(.x):not(.circle):hover::before,
  .board.circle .cell:not(.x):not(.circle):hover::after {
    content: '';
    position: absolute;
    border-radius: 50%;
  }
  .cell.circle::before,
  .board.circle .cell:not(.x):not(.circle):hover::before {
    width: var(--mark-size);
    height: var(--mark-size);
    background-color: black;
  }
  .cell.circle::after,
  .board.circle .cell:not(.x):not(.circle):hover::after {
    width: calc(var(--mark-size) * 0.7);
    height: calc(var(--mark-size) * 0.7);
    background-color: white;
  }

  .winning-message {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.8);
    display: none;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    color: white;
    font-size: 5rem;
  }

  .winning-message button {
    font-size: 3rem;
    background-color: white;
    border: 1px solid black;
    border-radius: 10px;
    padding: 0.25em 0.5rem;
    cursor: pointer;
    outline: none;
    transition: all 0.5s ease-out;
  }
  .winning-message button:hover {
    background-color: black;
    color: white;
    border-color: white;
    transition: all 0.5s ease-in;
  }

  .winning-message.show {
    display: flex;
  }
</style>
