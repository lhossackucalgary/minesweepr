<template lang="html">
  <div
    class="tile"
    ref="tile"
    v-bind:style="{backgroundColor: this.color}"
    @mouseover="mousemove"
    @mouseleave="setColor"
    @click.right.prevent="flagSwap"
    @click.left.prevent="clicked"
  >
  <div v-bind:class="{visible: this.isCleared}" class="textbox">
    <p>{{ this.numAdjacentBombs }}</p>
  </div>
  <img src="@/assets/flag-opt.svg" v-bind:class="{visible: this.isFlagged}" alt="flag">
  </div>
</template>

<script>
import EventBus from "@/store/EventBus.js";

export default {
  name: "Tile",
  data () {
    return {
      isFlagged: false,
      isOdd: false,
      isGameWon: false,
      isGameLost: false,
      color: "rgb(170,215,81)"
    }
  },
  props: {
    x: Number,
    y: Number,
    isBomb: Boolean,
    isCleared: Boolean,
    numAdjacentBombs: Number
  },
  watch: {
    x: function() {this.moved()},
    y: function() {this.moved()},
    isCleared: function () {
      this.setColor();
      // console.log(this.$refs["tile"].clientHeight);
    }
  },
  created: function() {
    this.reset();
    EventBus.$on('setDifficulty', this.reset);
    this.setOdd();
  },
  methods: {
    clicked () {
      if (this.isGameWon || this.isGameLost) {
        return;
      }
      if (!this.isFlagged) {
        EventBus.$emit('tileclick', {x: this.x, y: this.y})
        // this.isCleared = true;
      } else {
        // ignore clicks on flagged squares
      }
    },
    moved() {
      this.setOdd();
      this.setColor();
    },
    mousemove() {
      if (!this.isCleared && !this.isGameWon && !this.isGameLost) {
        this.color = "rgb(185, 221, 119)";
      }
    },
    flagSwap() {
      if (this.isGameWon || this.isGameLost) {
        return;
      }
      if (!this.isCleared) {
        if (this.isFlagged === true) {
          this.isFlagged = false;
          EventBus.$emit('rm-flag');
        } else if (this.isFlagged === false) {
          this.isFlagged = true;
          EventBus.$emit('add-flag');
        }
      }
    },
    setOdd(){
      if ((this.x + this.y) % 2 <= 0.5) {
        this.isOdd = true;
      } else {
        this.isOdd = false;
      }
    },
    reset() {
      this.isFlagged = false;
      this.isGameWon = false;
      this.isGameLost = false;
    },
    setColor() {
      if (this.isGameWon && this.isCleared) {
        this.color = "rgb(143,201,249)";
        return;
      }
      if (this.isGameLost && this.isBomb) {
        this.color = 'rgb(244,133,42)';
        return;
      }
      if (this.isOdd) {
        if (this.isCleared) {
          this.color = "rgb(215,184,153)";
        } else {
          this.color = "rgb(162,209,73)";
        }
      } else {
        if (this.isCleared) {
          this.color = "rgb(229,194,159)";
        } else {
          this.color = "rgb(170,215,81)";
        }
      }
    },
    showBombs() {
      this.isGameLost = true;
      if (this.isBomb) {
        this.setColor();
      }
    },
    gameWin() {
      this.isGameWon = true;
      this.setColor();
    }
  },
  mounted() {
    this.setOdd();
    this.setColor();
    EventBus.$on('lose-game', this.showBombs);
    EventBus.$on('win-game', this.gameWin);
  }
}
</script>

<style lang="css" scoped>
/* .tile:hover {
  background-color: rgb(185, 221, 119);
} */
img {
  display: block;
  width: 70%;
  margin: auto;
  margin-top: 10%;
  visibility: hidden;
}
.textbox {
  visibility: hidden;
  position: relative;
}
.visible {
  visibility: visible;
}
p {
  position: absolute;
  text-align: center;
  width: 100%;
}
</style>
