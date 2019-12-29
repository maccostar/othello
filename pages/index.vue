<template>
  <div class="board">
    <template v-for="y in board.length">
      <div
        v-for="x in board[y - 1].length"
        :key="`${x}/${y}`"
        @click="onClick(x - 1,y - 1)"
        class="cell"
      >
        <div
          v-if="board[y - 1][x - 1]"
          :class="['disc', board[y - 1][x - 1] === 1 ? 'black' : 'white']"
        />
      </div>
    </template>
  </div>
</template>

<script>
export default {
  data () {
    return {
      board: [
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 1, -1, 0, 0, 0],
        [0, 0, 0, -1, 1, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0]
      ],

      turn: 1
    }
  },

  methods: {
    onClick (x, y) {
      console.log('現在地e:', x, y)
      this.board = JSON.parse(JSON.stringify(this.board))
      const { board, turn } = this

      const directions = [[-1, -1], [0, -1], [1, -1], [-1, 0], [1, 0], [-1, 1], [0, 1], [1, 1]]

      const aroundCells = directions.filter(
        direction => board[y + direction[1]][x + direction[0]] === this.turn * -1
      )

      if (aroundCells.length > 0) {
        console.log('そこには置けます。')
        for (let i = 0; i < aroundCells.length; i++) {
          console.log(aroundCells[i])
        }
      } else {
        console.log('そこには置けません。')
      }

      console.log(aroundCells)
      board[y][x] = turn

      this.turn *= -1
    }
  }
}
</script>

<style scoped>
.board {
  width: 640px;
  height: 640px;;
  margin:  20px auto;
  background: #070;
}

.cell {
  width: 12.5%;
  height: 12.5%;
  float: left;
  border: 1px solid #000;
}

.disc{
  width:  80%;
  height: 80%;
  margin:  10%;
  border-radius: 50%;
}

.black{
  background: #000;
}

.white{
  background: #fff;
}

</style>
