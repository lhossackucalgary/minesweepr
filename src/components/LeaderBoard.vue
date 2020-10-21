<template lang="html">
  <div class="leaderboard" v-bind:style="{visibility: this.visibility}">
    <div class="leaders">
      <div class="curscore">
        <img src="@/assets/stopwatch-low-opt.svg" alt="Timer">
        <p>{{ this.curScore }}</p>
      </div>
      <div class="topscore">
        <img src="@/assets/trophy-opt.svg" alt="Trophy">
        <p>{{ this.topScore }}</p>
      </div>
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
      curScore: "----",
      visibility: "hidden",
      blocked: false
    }
  },
  methods: {
    show() {
      if (this.blocked) {
        this.blocked = false;
        return;
      }
      this.visibility = "visible";
    },
    hide() {
      this.visibility = "hidden";
    },
    showSoon() {
      setTimeout(() => {this.show()}, 2000);
    },
    blockLeaderboard() {
      this.blocked = true;
      this.visibility = "hidden";
      setTimeout(() => {
        this.blocked = false;
      }, 2000);
    },
    checkTopScore(s) {
      this.curScore = s;
      if (this.topScore === "----") {
        this.topScore = s;
      } else if (s < this.topScore) {
        this.topScore = s;
      }
    },
    restart() {
      this.hide();
      this.curScore = "----";
      EventBus.$emit('restart');
    }
  },
  mounted() {
    EventBus.$on('win-game', this.showSoon);
    EventBus.$on('lose-game', this.showSoon);
    EventBus.$on('game-win-time', this.checkTopScore);
    EventBus.$on('setDifficulty', this.blockLeaderboard);
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
  background-color: rgb(77,193,249);
  margin-bottom: 10px;
  border-radius: 8px;

  font-size: 2em;
  color: white;

  display: flex;
  flex-flow: row nowrap;
  justify-content: space-around;
  align-items: center;
}
button {
  padding: 1em;
  width: 500px;
  border-radius: 8px;
  background-color: rgb(74,117,44);
  color: white;
  font-size: 1.5em;
}
img {
  height: 3em;
}

@media (max-width: 800px) {
  button, .leaders {
    width: 80%;
  }
}
@media (max-height: 600px) and (orientation: landscape) {
  .leaders {
    height: 60vh;
  }
}

</style>
