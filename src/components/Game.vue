<template>
  <div class="game">
    <div class="game-area">
      <div class="game-title">
        <img src="/logo.svg" alt="Vue" />
        <h1>Jogo da Velha</h1>
      </div>

      <board :squares="squares" :winner="winner" @click="click" />

      <div class="game-info">
        <p v-if="stepNumber === 0">
          Vamos começar! Vai lá&nbsp;<b :class="currentPlayer">{{ currentPlayer }}</b>!
        </p>
        <p v-else-if="!!winner">
          O vencedor foi:&nbsp;
          <b :class="currentPlayer">{{ currentPlayer }}</b>!&nbsp;
          <button @click="restart">Jogar novamente</button>
        </p>
        <p v-else-if="stepNumber > 8">
          Deu velha!&nbsp;
          <button @click="restart">Jogar novamente</button>
        </p>
        <p v-else>
          Agora é a vez do jogador&nbsp;
          <b :class="currentPlayer">{{ currentPlayer }}</b>!&nbsp;atuar.
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  components: {
    Board: () => import('./Board')
  },
  data () {
    return {
      squares: Array(9).fill(null),
      stepNumber: 0,
      currentPlayer: 'X',
      winner: null
    }
  },
  methods: {
    hasWinner() {
      if (this.winner) return true

      const squares = this.squares
      const matches = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8], [0, 3, 6], [1, 4, 7],
        [2, 5, 8], [0, 4, 8], [2, 4, 6]
      ]

      for (let i = 0; i < matches.length; i++) {
        const [a, b, c] = matches[i]
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
          this.winner = [ a, b, c ]
          return true
        }
      }

      return false
    },

    restart() {
      this.squares = Array(9).fill(null)
      this.stepNumber = 0
      this.currentPlayer = this.currentPlayer
      this.winner = null
    },

    click (i) {
      if (this.squares[i] || this.winner) return
      this.$set(this.squares, i, this.currentPlayer)
      if (!this.hasWinner()) {
        this.stepNumber++
        this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X'
      }
    }
  }
}
</script>

<style scoped>
.game {
  background-color: rgba(var(--gradient-color-base));
  background-image: repeating-linear-gradient(45deg, #0000 5px, rgba(var(--gradient-color-1), .2) 5px, rgba(var(--gradient-color-1), .2) 10px, rgba(var(--gradient-color-2), 0) 10px, rgba(var(--gradient-color-2), 0) 35px, rgba(var(--gradient-color-2), .4) 35px, rgba(var(--gradient-color-2), .4) 40px,rgba(var(--gradient-color-1), .2) 40px, rgba(var(--gradient-color-1), .2) 50px, rgba(10, 36, 45, 0) 50px, rgba(var(--gradient-color-1), 0) 60px,rgba(var(--gradient-color-2), .4) 60px, rgba(var(--gradient-color-2), .4) 70px, rgba(var(--gradient-color-3), .3) 70px, rgba(var(--gradient-color-3), .3) 80px,rgba(var(--gradient-color-3), 0) 80px, rgba(var(--gradient-color-3), 0) 90px, rgba(var(--gradient-color-2), .4) 90px, rgba(var(--gradient-color-2), .4) 110px,rgba(var(--gradient-color-2), 0) 110px, rgba(var(--gradient-color-2), 0) 120px, rgba(var(--gradient-color-1), .2) 120px, rgba(var(--gradient-color-1), .2) 140px), repeating-linear-gradient(135deg, #0000 5px, rgba(var(--gradient-color-1), .2) 5px, rgba(var(--gradient-color-1), .2) 10px,rgba(var(--gradient-color-2), 0) 10px, rgba(var(--gradient-color-2), 0) 35px, rgba(var(--gradient-color-2), .4) 35px, rgba(var(--gradient-color-2), .4) 40px,rgba(var(--gradient-color-1), .2) 40px, rgba(var(--gradient-color-1), .2) 50px, rgba(var(--gradient-color-1), 0) 50px, rgba(var(--gradient-color-1), 0) 60px,rgba(var(--gradient-color-2), .4) 60px, rgba(var(--gradient-color-2), .4) 70px, rgba(var(--gradient-color-3), .3) 70px, rgba(var(--gradient-color-3), .3) 80px,rgba(var(--gradient-color-3), 0) 80px, rgba(var(--gradient-color-3), 0) 90px, rgba(var(--gradient-color-2), .4) 90px, rgba(var(--gradient-color-2), .4) 110px,rgba(var(--gradient-color-2), 0) 110px, rgba(var(--gradient-color-2), 0) 140px, rgba(var(--gradient-color-1), .2) 140px, rgba(var(--gradient-color-1), .2) 160px);
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-area {
  display: flex;
  flex-flow: column;
}

.game-title {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 0 3vmin;
}

.game-title img {
  margin: 0 12px 0 -20px;
  width: 40px;
  filter: drop-shadow(-1px 1px 0 #0007) drop-shadow(1px -1px 0 #0007) drop-shadow(1px 1px 0 #0007);
}

.game-title h1 {
  margin: 0;
  font-size: 2.25em;
  color: rgba(var(--theme-color));
  text-shadow: -1px -1px 1px #000b, -1px 1px 1px #000b, 1px -1px 1px #000b, 1px 1px 1px #000b;
}

.game-info {
  margin: 3vmin 0 0;
  padding: 1rem .5rem;
  font-size: 1.25em;
  text-align: center;
  box-shadow: 2.5px 5px 25px #0001, 0 1px 6px #0004;
  text-shadow: 0 0 1px #fff, 0 2px 5px #fff5;
  border-radius: .5rem;
  backdrop-filter: blur(10px);
  background: #fff6;
  background-blend-mode: exclusion;
  background-image: var(--noise-pattern);
  color: #111;
}

.game-info p {
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.game-info .X,
.game-info .O {
  text-shadow: -1px -1px 0 #000b, -1px 1px 0 #000b, 1px -1px 0 #000b, 1px 1px 0 #000b;
}

.game-info .X {
  color: #ff5722;
}

.game-info .O {
  color: #ffeb3b;
}

.game-info button {
  text-transform: uppercase;
  font-weight: 600;
  font-size: .75em;
  padding: .5rem 1rem;
  margin: -.5rem 0 -.5rem 1rem;
  border: 2px solid #fff;
  background: #fff5;
  text-shadow: 0 0 1px #fff, 0 2px 5px #fff5;
  color: #111;
  cursor: pointer;
  outline: none;
  transition: all .25s ease;
}

.game-info button:focus,
.game-info button:hover {
  background: #1115;
  border-color: rgba(var(--theme-color));
  box-shadow: 0 0 10px rgba(var(--theme-color), .75);
  color: rgba(var(--theme-color));
  text-shadow: -1px -1px 0 #0007, -1px 1px 0 #0007, 1px -1px 0 #0007, 1px 1px 0 #0007;
}

.game-info button:active {
  background: #1119;
}
</style>