<template>
  <div>
    Rows:
    <input type="number" v-model.number="rows">
    Cols:
    <input type="number" v-model.number="cols">
    Mines:
    <input type="number" v-model.number="minesCount">
    <button @click='newGame'>New Game</button>
  </div>
</template>

<script>
export default{
  data(){
    return {
      rows: 15,
      cols: 15,
      minesCount: 50
    }
  },
  methods: {
    newGame(){
      const game = {
        rows: this.rows,
        cols: this.cols,
        minesCount: this.minesCount
      }
      this.$emit('newGame', game)
    }
  }
}
</script>

<style>

</style>