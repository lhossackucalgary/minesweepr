<template lang="html">
  <div
    class="tile"
    v-bind:style="{backgroundColor: this.color}"
    @mouseover="mousemove"
    @mouseleave="setColor"
    @click.right.prevent="flagSwap"
    @click.left.prevent="clicked"
  >
  <img src="@/assets/flag-opt.svg" v-bind:class="{flagged: this.isFlagged}" alt="flag">
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
    y: function() {
      this.setOdd();
      this.setColor();
    },
    isCleared: function () {
      this.setColor();
    }
  },
  created: function() {
    this.reset();
    EventBus.$on('setDifficulty', this.reset);
    this.setOdd();
  },
  methods: {
    clicked () {
      if (!this.isFlagged) {
        EventBus.$emit('tileclick', {x: this.x, y: this.y})
        // this.isCleared = true;
      } else {
        // ignore clicks on flagged squares
      }
    },
    mousemove() {
      if (!this.isCleared) {
        this.color = "rgb(185, 221, 119)";
      }
    },
    flagSwap() {
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
      // this.isCleared = false;
    },
    setColor() {
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
    }
  },
  mounted() {
    this.setOdd();
    this.setColor();
  }
}
</script>

<style lang="css" scoped>
.tile:hover {
  background-color: rgb(185, 221, 119);
}
img {
  display: block;
  width: 70%;
  margin: auto;
  margin-top: 10%;
  visibility: hidden;
}
.flagged {
  visibility: visible;
}
</style>
