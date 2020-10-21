<template lang="html">
  <div
    class="tile"
    ref="tile"
    v-bind:style="{backgroundColor: this.bgcolor}"
    @mouseover="mousemove"
    @mouseleave="setColor"
    @click.right.prevent="flagSwap"
    @click.left.prevent="clicked"
  >
  <p v-bind:class="{visible: this.isCleared}"
     v-bind:style="{fontSize: this.height}"
    >{{ this.numAdjacentBombs }}</p>
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
      bgcolor: "rgb(170,215,81)",
      color: "black",
      height: "1em"
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
    isBomb: function() {this.moved()},
    isCleared: function () {
      this.setColor();
      // console.log(this.$refs["tile"].clientHeight);
    },
    numAdjacentBombs: function() {

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
    resize() {
      this.height = String(this.$refs.tile.clientHeight) + "px";
    },
    moved() {
      this.setOdd();
      this.setColor();
      this.resize();
    },
    mousemove() {
      if (!this.isCleared && !this.isGameWon && !this.isGameLost) {
        this.bgcolor = "rgb(185, 221, 119)";
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
      this.resize();
      if (this.isGameWon && this.isCleared) {
        this.bgcolor = "rgb(143,201,249)";
        return;
      }
      if (this.isGameLost && this.isBomb) {
        this.bgcolor = 'rgb(244,133,42)';
        return;
      }
      if (this.isOdd) {
        if (this.isCleared) {
          this.bgcolor = "rgb(215,184,153)";
        } else {
          this.bgcolor = "rgb(162,209,73)";
        }
      } else {
        if (this.isCleared) {
          this.bgcolor = "rgb(229,194,159)";
        } else {
          this.bgcolor = "rgb(170,215,81)";
        }
      }
    },
    showBombs() {
      this.isGameLost = true;
      if (this.isBomb && !this.isFlagged) {
        this.setColor();
      }
      if (this.isFlagged && !this.isBomb) {
        this.bgcolor = "black";
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
    EventBus.$on('setDifficulty', this.setColor);
    this.resize();
    window.addEventListener('resize', this.resize);
  }
}
</script>

<style lang="css" scoped>
.tile {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}
img {
  display: block;
  width: 70%;
  margin: auto;
  margin-top: 10%;
  visibility: hidden;
}
/* .textbox {
} */

p {
  position: absolute;
  visibility: hidden;
}
.visible {
  visibility: visible;
}
</style>
