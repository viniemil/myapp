<script>
import Square from './components/Filed.vue'
export default {
  name: "app",
  components: { Square },
  beforeCreate() {
    localStorage.clear()
  },
  data() {
    return {
      xIsNext: true,
      restartGame: false,
      squareValue: Array(9).fill(null),
      status: { status: false, msg: '', xWiner: false, oWiner: false, draw: false },
      playCount: {
        mCount: localStorage.getItem('mCount') ? localStorage.getItem('mCount') : 0,
        xCount: localStorage.getItem('xCount') ? localStorage.getItem('xCount') : 0,
        oCount: localStorage.getItem('oCount') ? localStorage.getItem('oCount') : 0,
      },
      lines: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ]
    }
  },
  methods: {
    computeStatus() {
      if (this.calculateWinner() === 'X' || this.calculateWinner() === 'O') {
        this.status.msg = "Winner: " + this.calculateWinner();
        this.status.status = true
        this.restartGame = true
        this.setIsWinner()
      } else {
        this.status.msg = "Próximo a Jogar: " + (this.xIsNext ? "X" : "O");
      }

      if (this.calculateWinner() === "Draw") {
        this.status.msg = this.calculateWinner()
        this.restartGame = true
      }
      return this.status.msg;
    },
    calculateWinner() {

      const values = []


      for (let i = 0; i < this.lines.length; i++) {
        const [a, b, c] = this.lines[i];
        values.push(this.squareValue[a], this.squareValue[b], this.squareValue[c])
        if (
          this.squareValue[a] &&
          this.squareValue[a] === this.squareValue[b] &&
          this.squareValue[a] === this.squareValue[c]
        ) {
          return this.squareValue[a];
        }

      }

      this.status.draw = !values.includes(null);

      if (this.status.draw) {
        return "Draw"
      }

      return null;
    },
    onActionFill(i) {
      if (this.squareValue[i]) {
        return
      }
      if (this.xIsNext) {
        this.squareValue[i] = "X"
      } else {
        this.squareValue[i] = "O"
      }

      this.setXIsNext()
      return this.squareValue
    },
    setXIsNext() {
      this.xIsNext = !(this.xIsNext);
    },
    setIsWinner() {
      if (this.calculateWinner() === 'X') {
        this.status.xWiner = true
      }
      if (this.calculateWinner() === 'O') {
        this.status.oWiner = true
      }
    },
    newGame() {
      this.status.xWiner ? this.playCount.xCount = this.playCount.xCount + 1 : this.playCount.xCount;
      this.status.oWiner ? this.playCount.oCount = this.playCount.oCount + 1 : this.playCount.oCount;
      this.playCount.mCount = this.playCount.mCount + 1
      localStorage.setItem('mCount', this.playCount.mCount)
      localStorage.setItem('xCount', this.playCount.xCount)
      localStorage.setItem('oCount', this.playCount.oCount)
      this.squareValue = Array(9).fill(null);
      this.restartGame = false;
      this.xIsNext = true;
      this.status.status = false
      this.status.xWiner = false
      this.status.oWiner = false
      this.status.draw = false
    },
    getClass() {
      if (this.status.status) {

        return "showStatusWinner"
      }

      return "showStatus"
    }
  },
}
</script>

<template>
  <div class="centerpage">
    <h1 class="centerTitle">Jogo da Velha</h1>
    <p v-bind:class='getClass()'>{{ computeStatus() }}</p>
    <button class="restartButton" v-if="restartGame" @click="newGame">Restart Game</button>
    <div class="row">
      <Square :squareValue="squareValue[0]" @actionFill="onActionFill(0)" />
      <Square :squareValue="squareValue[1]" @actionFill="onActionFill(1)" />
      <Square :squareValue="squareValue[2]" @actionFill="onActionFill(2)" />
    </div>
    <div class="row">
      <Square :squareValue="squareValue[3]" @actionFill="onActionFill(3)" />
      <Square :squareValue="squareValue[4]" @actionFill="onActionFill(4)" />
      <Square :squareValue="squareValue[5]" @actionFill="onActionFill(5)" />
    </div>
    <div class="row">
      <Square :squareValue="squareValue[6]" @actionFill="onActionFill(6)" />
      <Square :squareValue="squareValue[7]" @actionFill="onActionFill(7)" />
      <Square :squareValue="squareValue[8]" @actionFill="onActionFill(8)" />
    </div>
  </div>
  <div class="bottompage">Partidas : {{ playCount.mCount }} | Score X: {{ playCount.xCount }} - Score O: {{
    playCount.oCount }}</div>
</template>


<style scoped>
.centerpage {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.bottompage {
  position: absolute;
  bottom: 0;
  /* margin-bottom: 10px; */
  padding: 20px 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  text-align: center;
  /* padding: 4px 4px; */
  background-color: #f4f4f4;
}

.square {
  background-color: #fff;
  width: 80px;
  height: 80px;
  border: 1px solid #83989e;
  text-align: center;
  font-weight: bold;
  font-size: 32px;
  line-height: 80px;
}

.showStatus {
  background-color: #f6f6f6;
  padding: 6px 10px;
  text-align: center;
}

.showStatusWinner {
  background-color: #70f156;
  padding: 6px 10px;
  text-align: center;
}

.restartButton {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-bottom: 10px;
  padding: 8px 8px;
  background-color: #f6f6f6;
  border: none;
  font-size: 20px;
  font-weight: bold;
  color: green;
}

.row {
  display: flex;
  justify-content: center;
}

.centerTitle {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>