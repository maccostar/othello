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
    <p> {{ turn === 1 ? '黒' : '白' }}の番です </p>

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

      const directionsToGo = directions.filter(
        direction =>
          x + direction[0] >= 0 &&
          y + direction[1] >= 0 &&
          x + direction[0] < 8 &&
          y + direction[1] < 8 &&
          board[y + direction[1]][x + direction[0]] === this.turn * -1
      ).map(
        direction => searchReturnDisk(direction, 1, this.turn)
      )
      console.log(directionsToGo)

      for (let i = 0; i < directionsToGo.length; i++) {
        if (directionsToGo[i].length === 2) {
          for (let step = 1; step < directionsToGo[i][1]; step++) {
            const _x = x + directionsToGo[i][0][0] * step
            const _y = y + directionsToGo[i][0][1] * step
            console.log(x, _x, y, _y, this.turn)
            board[_y][_x] = this.turn
          }
        }
      }

      function searchReturnDisk (direction, step, turn) {
        const _x = x + direction[0] * step
        const _y = y + direction[1] * step
        console.log(_x, _y)

        const isOnBoard = _x >= 0 && _x < 8 && _y >= 0 && _y < 8
        if (!isOnBoard) { return 'none' }

        if (board[_y][_x] === turn * -1) {
          step += 1
          return searchReturnDisk(direction, step, turn)
        } else if (board[_y][_x] === turn && step > 1) {
          return [ direction, step ]
        } else {
          return 'none'
        }
      }

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
