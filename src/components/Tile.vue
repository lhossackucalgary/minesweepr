<template lang="html">
  <div
    class="tile"
    v-bind:class="{isOdd: this.isOdd}"
    @click.right.prevent="flagSwap"
  >
  <img src="@/assets/flag-opt.svg" v-bind:class="{flagged: this.isFlagged}" alt="flag">
  </div>
</template>

<script>
export default {
  name: "Tile",
  data () {
    return {
      isFlagged: false,
      isOdd: false
    }
  },
  props: {
    x: Number,
    y: Number,
    isBomb: Boolean,
    size: Number
  },
  watch: {
    x: function() {
      this.setColor();
    }
  },
  created: function() {
    this.setColor();
  },
  methods: {
    clicked (e) {
      console.log('clicked');
      console.log(e);
    },
    flagSwap() {
      if (this.isFlagged === true) {
        this.isFlagged = false;
      } else if (this.isFlagged === false) {
        this.isFlagged = true;
      }
    },
    setColor(){
      if ((this.x + this.y) % 2 <= 0.5) {
        this.isOdd = true;
      } else {
        this.isOdd = false;
      }
    }
  }
}
</script>

<style lang="css" scoped>
.tile {
  background-color: rgb(170,215,81);
}
.isOdd {
  background-color: rgb(162,209,73);
}
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
