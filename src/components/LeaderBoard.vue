<template lang="html">
  <div class="leaderboard" v-bind:style="{visibility: this.visibility}">
    <div class="leaders">

    </div>
    <button @click.left.prevent="restart">Restart</button>
  </div>
</template>

<script>
import EventBus from "@/store/EventBus.js";

export default {
  data: function () {
    return {
      topScore: "----",
      visibility: "hidden"
    }
  },
  methods: {
    show() {
      this.visibility = "visible";
    },
    hide() {
      this.visibility = "hidden";
    },
    showSoon() {
      setTimeout(() => {this.show()}, 2000);
    },
    checkTopScore(s) {
      if (this.topScore === "----") {
        this.topScore = s;
      } else if (s < this.topScore) {
        this.topScore = s;
      }
    },
    restart() {
      this.hide();
      EventBus.$emit('restart');
    }
  },
  mounted() {
    EventBus.$on('win-game', this.showSoon);
    EventBus.$on('lose-game', this.showSoon);
    EventBus.$on('game-win-time', this.checkTopScore);

  }
}
</script>

<style lang="css" scoped>
.leaderboard {
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(0, 0, 0, 0.7);
  z-index: 99;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.leaders {
  width: 500px;
  height: 35vh;
  background-color: rgb(143,201,249);
  margin-bottom: 10px;
  border-radius: 8px;
}
button {
  padding: 1em;
  width: 500px;
  border-radius: 8px;
  background-color: rgb(74,117,44);
  color: white;
  font-size: 1.5em;
}

@media (max-width: 800px) {
  button, .leaders {
    width: 80%;
  }
}
@media (orientation: landscape) {
  .leaders {
    height: 60vh;
  }
}

</style>
