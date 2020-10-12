<template>
  <div class="game-grid" v-bind:class="{easy: this.difficulty === 'Easy', medium: this.difficulty === 'Medium'}">
    <Tile v-for="(tile, index) in tiles" v-bind:key="index" v-bind:x="getX(index)" v-bind:y="getY(index)">
    </Tile>
  </div>
</template>

<script>
import Tile from "@/components/Tile.vue";
import EventBus from "@/store/EventBus.js";

export default {
  name: 'Gameboard',
  components: {
    Tile
  },
  data () {
    return {
      tiles: [1,2,3,4,5,6,7,8,9,10],
      difficulty: "Medium",
      difficultyParams: {
        "Easy": {
          xLen: 10,
          yLen: 8,
          bombs: 10
        },
        "Medium": {
          xLen: 18,
          yLen: 14,
          bombs: 40
        }
      }
    }
  },
  computed: {
  },
  methods: {
    resetBoard(difficulty) {
      this.tiles = new Array(this.difficultyParams[difficulty].xLen*this.difficultyParams[difficulty].yLen);
      this.difficulty = difficulty;
    },
    getX(index){
      return index % this.difficultyParams[this.difficulty].xLen;
    },
    getY(index){
      return Math.floor(index / this.difficultyParams[this.difficulty].xLen);
    }
  },
  mounted() {
    EventBus.$on('setDifficulty', this.resetBoard)
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
