<template>
  <div id="app">
    <ul id="board" class="white normal">
      <li
        class="square"
        v-for="(item, index) in datas"
        :key="index"
        @click="handle(index)"
      >
        {{ item }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      datas: Array(9).fill(""),
      history: [],
      next: true,
      winner: "",
      cases: [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ],
    };
  },

  methods: {
    handle(i) {
      if (!this.datas[i] && !this.winner) {
        this.$set(this.datas, i, this.next_player);
        this.history.push({
          status: [...this.datas],
          player: this.next,
        });
        if (this.is_win(this.next_player)) {
          this.winner = this.next_player;
        }
        this.next = !this.next;
      }
    },
    //跳转到第n步
    jump(idx) {
      this.datas = this.history[idx].status;
      this.history.splice(idx + 1, this.history.length - idx - 1);
      this.next = !this.history[idx].player;
      this.winner = this.is_win("O") ? "O" : this.is_win("X") ? "X" : "";
    },
    //判断胜出
    is_win(player) {
      return this.cases.some((arr) =>
        arr.every((el) => this.datas[el] === player)
      );
    },
    //初始化
    init() {
      this.datas = Array(9).fill("");
      this.history = [];
      this.next = true;
      this.winner = "";
    },
  },

  computed: {
    next_player() {
      return this.next ? "O" : "X";
    },
    hint() {
      return this.winner
        ? "winner: " + this.winner
        : "next: " + this.next_player;
    },
  },
};
</script>

<style>
body {
  background: yellowGreen;
}

.white {
  background: #fff;
  border-radius: 6px;
  outline: none;
  border: none;
}

.normal {
  list-style: none;
  padding: 0px;
  margin: 0px;
}

#app {
  display: flex;
  justify-content: space-between;
  width: 450px;
  height: 306px;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}

#board {
  display: flex;
  width: 306px;
  height: 306px;
  flex-wrap: wrap;
  overflow: hidden;
}

#hint {
  width: 100px;
  height: 22px;
  text-align: center;
  margin: 10px;
}

#restart {
  width: 70px;
  height: 22px;
  margin: 10px;
}

#history， .history {
  margin: 5px;
}

.square {
  height: 100px;
  width: 100px;
  border: #ebebeb solid 1px;
  flex: 0 0 auto;
  font-size: 50px;
  font-weight: 900;
  line-height: 100px;
  text-align: center;
}
</style>