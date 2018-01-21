<template>
  <div>
    <grid-score ref='gridScore'></grid-score>
    <table id='minesweeper'>
      <tr v-for='row in game.rows'>
        <td class="unopened"
            v-for='col in game.cols'
            @click="openTile(row, col)"
            @click.right.prevent="flagTile(row, col, $event)"
            :ref="`r${row}c${col}`"
            :id="`r${row}c${col}`"></td>
      </tr>
    </table>
  </div>
</template>

<script>

import gridScore from './GridScore.vue'
const neighbours = [[-1,-1],[-1,0],[-1,1],[0,-1],[0,1],[1,-1],[1,0],[1,1]]

export default{
  props: ['game'],
  data(){
    return {
      mines: [],
      isRunning: false
    }
  },
  methods: {
    openTile(row, col){
      let tile = null
      const ref = `r${row}c${col}`
      if(this.$refs[ref] && (this.$refs[ref][0].className == 'unopened')){
        tile = this.$refs[ref][0]
      }
      else{
        return null
      }

      // If is the first click, start timer
      if(!this.isRunning){
        this.isRunning = true
        this.$refs.gridScore.startTimer()
      }


      if (this.hasMine(row, col)) {
        tile.className='mine'
        this.gameOver()
      } else {
        const count = this.countNeighborsMines(row, col);
        if (count == 0) {
          tile.className = 'opened'
          this.openEmptyTiles(row, col)
        } else {
          tile.className = `mine-neighbour-${count}`
        }
      }
    },
    flagTile(){
      if(event.target.className == 'flagged' || event.target.className == 'unopened'){
        event.target.className = event.target.className == 'flagged' ? 'unopened' : 'flagged'
      }
    },
    plantMines(){
      for (let m = 0; m < this.game.minesCount; m += 1) {
        const row = Math.floor(Math.random() * this.game.rows) + 1;
        const col = Math.floor(Math.random() * this.game.cols) + 1;
        this.mines.push([row, col]);
      }
    },
    hasMine(row, col) {
      let mine = false;
      this.mines.forEach((m) => {
        if (m[0] === row && m[1] === col) { mine = true }
      });
      return mine;
    },
    countNeighborsMines(row, col) {
      let count = 0;
      neighbours.forEach((pos) => {
        if (this.hasMine(row + pos[0], col + pos[1])){
          count += 1
        }
      })
      return count;
    },
    openEmptyTiles(row, col){
      neighbours.forEach((pos) => {
        this.openTile(row + pos[0], col + pos[1])
      })
    },
    gameOver(){
      this.openAllTiles()
      this.$refs.gridScore.stopTimer()
      this.isRunning = false
    },
    openAllTiles(){
      for(let row=1; row <= this.game.rows; row++){
        for(let col=1; col <= this.game.cols; col++){
          this.openTile(row,col)
        }
      }
    }
  },
  created(){
    this.plantMines();
  },
  components: {
    gridScore
  }
}
</script>

<style></style>