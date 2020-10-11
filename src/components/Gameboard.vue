<template>
  <div class="game-grid" v-bind:class="{easy: this.difficulty == 0, medium: this.difficulty == 1}">
    <Tile v-for="(tile, index) in tiles" v-bind:key="index" v-bind:position="index">
    </Tile>
  </div>
</template>

<script>
import Tile from "@/components/Tile.vue";

export default {
  name: 'Gameboard',
  components: {
    Tile
  },
  data () {
    return {
      tiles: [1,2,3,4,5,6,7,8,9,10],
      difficulty: 0
    }
  },
  computed: {
  }
}
</script>

<style media="screen">
.game-grid {
  display: grid;
  width: 100vw;
}

.easy {
  height: 80vw;
  grid-template-columns: repeat(10, 1fr);
  grid-template-rows: repeat(8, 1fr);
}

.medium {
  height: 77.778vw;
  grid-template-columns: repeat(18, 1fr);
  grid-template-rows: repeat(14, 1fr);
}

@media (min-width: 800px) {
  .game-grid {
    width: 800px;
    align-self: center;
  }
  .easy {
    height: 640px;
  }
  .medium {
    height: 622.23px;
  }
}
</style>
