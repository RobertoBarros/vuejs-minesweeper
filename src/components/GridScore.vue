<template>
  <div>
    <h2>{{timer}}</h2>
  </div>
</template>

<script>
export default{
  data(){
    return {
      gameStart: null,
      elapsedTime: null,
      windowTimer: null,
    }
  },
  methods: {
    startTimer() {
      console.log('START TIMER')
      this.gameStart = new Date().getTime();
      this.windowTimer = setInterval(() => {
        this.elapsedTime = (new Date().getTime() - this.gameStart);
      }, 1000);
    },
    stopTimer() {
      clearInterval(this.windowTimer);
      this.windowTimer = null;
    }
  },
  computed: {
    timer() {
      let totalSeconds = Math.floor(this.elapsedTime/1000);
      let minutes = Math.floor(totalSeconds/60);
      let seconds = totalSeconds - (minutes * 60);
      if (minutes.toString().length == 1) {
        minutes = '0' + minutes;
      }
      if (seconds.toString().length == 1) {
        seconds = '0' + seconds;
      }
      return minutes + ':' + seconds;
    },
  }
}
</script>

<style scoped>
  h2 {
    text-align: center;
  }
</style>