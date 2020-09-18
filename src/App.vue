<template>
  <div id="app">
    <div>
      <div class="board-container flex-container flex-column flex-center">
        <div class="flex-container">
          <div class="title">Tic Tac Toe Game</div>
        </div>
        <div class="flex-container flex-center">
          <div class="winner">
            <div v-if="win" class="blueviolet">
              {{winner}}
            </div>
            <div v-else-if="currentTurn === 9" class="blueviolet">Draw</div>
            <div v-else-if="currentTurn%2 === 0" class="red">X turn</div>
            <div v-else class="green">O turn</div>
          </div>
        </div>
        <button class="reset" onclick="window.location.reload();">Reset</button>
        <div class="board flex-container flex-wrap">
          <div v-for="(board,id) in boards" :key="id" class="square flex-container flex-center" @click="play(board,id)">
            <div :class="{red: board.X}">{{board.val}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      win: false,
      plays: undefined,
      winner: "",
      player1: "X",
      player2: "0",
      boards: [
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
        { val: "" },
      ],
      playedboard: [],
      winning: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [6, 4, 2],
      ],
      currentTurn: 0
    };
  },
  methods: {
    play(board, id) {
      if (this.currentTurn === 9 || this.win) {
        return;
      } else if (this.currentTurn % 2 === 0 && board.val === "") {
        board.val = this.player1;
        board.X = "X";
        this.playedboard[id] = board.val;
        this.currentTurn++;
      } else if (board.val === "") {
        board.val = this.player2;
        this.playedboard[id] = board.val;
        this.currentTurn++;
      }
      if (this.currentTurn > 4) {
      this.checkwin(this.playedboard, this.player1);
      this.checkwin(this.playedboard, this.player2);
      }
    },
    checkwin(playedboard, player) {
      this.plays = playedboard.reduce(
        (a, e, i) => (e === player ? a.concat(i) : a),
        []
      );
      for (let [index, win] of this.winning.entries()) {
        if (win.every((elem) => this.plays.indexOf(elem) > -1)) {
          this.win = true
          this.winner = `${player} is the winner!`
          this.index = index
          break;
        }
      }
    },
  },
};
</script>

<style>
html,
#app {
  height: 100%;
  font-family: "Open Sans", sans-serif;
  background-color: gainsboro;
}

.title {
  font-size: 60px;
  color: black;
  margin-bottom: 10px;
}

.flex-container {
  display: flex;
}

.flex-column {
  flex-direction: column;
}

.flex-center {
  align-items: center;
  justify-content: center;
}

.flex-wrap {
  flex-wrap: wrap;
}

.board-container {
  height: 100%;
  width: 100%;
}

.board {
  width: 618px;
}

.square {
  width: 200px;
  height: 200px;
  border: 2px solid mediumslateblue;
  font-size: 56px;
  color: green;
}

.square:hover {
  background: floralwhite;
  cursor: pointer;
}

.winner {
  padding: 20px;
  font-size: 32px;
  color: black;
}

.reset {
  cursor: pointer;
  padding: 10px;
  margin-top: 10px;
  margin-bottom: 20px;
}

.red {
  color: red;
}

.blueviolet{
  color: blueviolet;
}
</style>
