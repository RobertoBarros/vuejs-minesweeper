<template>
  <div>
    <app-options @newGame='newGame'></app-options>
    <app-grid :game='game' :key='gridKey'></app-grid>
  </div>
</template>

<script>
import appGrid from './components/AppGrid'
import appOptions from './components/AppOptions'

export default {
  data () {
    return {
      game: {
        rows: 0,
        cols: 0,
        minesCount: 0
      },
      gridKey: 0,
    }
  },
  methods: {
    newGame(game){
      this.game = game
      this.gridKey += 1
    },
  },
  components: {
    appOptions,
    appGrid
  }
}
</script>

<style>
  @import './assets/styles/minesweeper.css'
</style>
