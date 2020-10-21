<template>
  <div class="timer-widget">
    <img src="@/assets/stopwatch-low-opt.svg" alt="Timer">
    <p id="time">{{ this.ellapsedTime.toString().padStart(3, '0') }}</p>
  </div>
</template>

<script>
import EventBus from "@/store/EventBus.js";

export default {
  name: "Timer",
  components: {
  },
  data () {
    return {
      ellapsedTime: 0,
      stopTimer: false,
      startTime: null
    }
  },
  methods: {
    resetTimer() {
      this.stop();
      this.ellapsedTime = 0;
    },
    start() {
      this.ellapsedTime = 0;
      this.startTime = Date.now();
      this.stopTimer = false;
      setTimeout(() => {
        this.inc();
      }, 1000);
    },
    stop() {
      this.stopTimer = true;
      this.ellapsedTime = parseInt((Date.now() - this.startTime)/1000);
    },
    inc() {
      if (!this.stopTimer) {
        this.ellapsedTime = parseInt((Date.now() - this.startTime)/1000);
        let clockDrift = (Date.now() - this.startTime) - (this.ellapsedTime * 1000);
        setTimeout(() => {
          this.inc();
        }, 1000-clockDrift);
      }
    },
    winGame() {
      this.stop();
      EventBus.$emit('game-win-time', this.ellapsedTime);
    }
  },
  mounted() {
      EventBus.$on('setDifficulty', this.resetTimer);
      this.resetTimer();
      EventBus.$on('lose-game', this.stop);
      EventBus.$on('win-game', this.winGame);
      EventBus.$on('start', this.start);
  }
}
</script>

<style scoped>
img {
  height: 1.5em;
}

.timer-widget {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
</style>
