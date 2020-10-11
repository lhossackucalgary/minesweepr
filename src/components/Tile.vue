<template lang="html">
  <div
    class="tile"
    v-bind:class="{isOdd: this.isOdd}"
    @click.right="clicked"
    @click="clicked"
  >
    {{ this.position }}
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
    position: Number,
    isBomb: Boolean,
    size: Number
  },
  // watch: {
  // },
  created: function() {
    if (this.position % 2 != 0) {
      this.isOdd = true;
    }
  },
  methods: {
    clicked (e) {
      console.log('clicked');
      console.log(e);
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
</style>
