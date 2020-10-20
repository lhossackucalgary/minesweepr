<template>
  <div class="game-grid" ref="board"
        v-bind:class="{easy: this.difficulty === 'Easy', medium: this.difficulty === 'Medium'}"
        v-bind:style="{
          height: this.boardHeight,
          gridTemplateColumns: this.colTemplate
        }"
        >
    <Tile v-for="(tile, index) in tiles"
          v-bind:key="index"
          v-bind:x="getX(index)"
          v-bind:y="getY(index)"
          v-bind:isBomb="tiles[index].isBomb"
          v-bind:isCleared="tiles[index].isCleared"
          v-bind:numAdjacentBombs="tiles[index].numAdjacentBombs"
          >
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
      tiles: [],
      difficulty: "",
      boardHeight: null,
      colTemplate: null,
      bombCount: 0,
      xLen: 0,
      yLen: 0,
      started: false
    }
  },
  watch: {
    bombCount: function() {
      EventBus.$emit('setBombCount', this.bombCount);
    }
  },
  methods: {
    resetBoard(difficulty) {
      // Set board template
      let vw = this.$refs["board"].clientWidth;
      let header_height = this.$parent.$children[0].$refs["header"].clientHeight;
      let vh = window.innerHeight - header_height;
      let aspect_ratio = vw/vh;
      this.difficulty = difficulty;

      let tile_target = 180;
      if (this.difficulty === "Easy") {
        this.bombCount = 10;
        if (vw < 600 || vh < 600) {
          tile_target = 80;
        } else {
          tile_target = 180;
        }
      } else if (this.difficulty === "Medium") {
        if (vw < 600 || vh < 600) {
          tile_target = 200;
          this.bombCount = 35;
        } else {
          tile_target = 252;
          this.bombCount = 40;
        }
      }

      // find a reasonable x-y size based on aspect_ratio..
      let xLen = 1;
      let yLen = 1;
      while (xLen * yLen < tile_target) {
        if (xLen/ yLen > aspect_ratio) {
          yLen += 1;
        } else {
          xLen += 1;
        }
      }
      if ( 0.95*vw/xLen * yLen >= vh) {
        yLen -= 1;
      }

      this.xLen = xLen;
      this.yLen = yLen;
      this.colTemplate = `repeat(${this.xLen}, 1fr)`;
      this.height = `${this.xLen/this.yLen}vw`

      this.tiles = new Array(this.xLen*this.yLen);
      for (let i = 0; i < this.tiles.length; i++) {
        this.tiles[i] = {
          isBomb: false,
          isCleared: false,
          numAdjacentBombs: null
        }
      }
      this.started = false;
    },
    getX(index){
      return index % this.xLen;
    },
    getY(index){
      return Math.floor(index / this.xLen);
    },
    getIndex(x,y){
      return y*this.xLen + x;
    },
    tileClicked(tilexy) {
      if (!this.started) {
        this.started = true;
        EventBus.$emit('start');
        this.gameboardGen(tilexy);
      }
      this.uncoverTiles(tilexy);
    },
    gameboardGen(tilexy) {
      // Place bombs randomly, but not on or beside first click
      let numBombs = 0;
      let indexNotAllowed = new Array();
      for (let i = -1; i < 2; i++) {
        for (let j = -1; j < 2; j++) {
          if (0 <= this.getIndex(i + tilexy.x, j + tilexy.y) &&
              this.getIndex(i + tilexy.x, j + tilexy.y) < this.xLen * this.yLen) {
            indexNotAllowed.push(this.getIndex(i + tilexy.x, j + tilexy.y));
          }
        }
      }
      while(numBombs < this.bombCount) {
        let bombIndex = Math.floor(Math.random()*this.xLen*this.yLen);
        if (!indexNotAllowed.includes(bombIndex) && !this.tiles[bombIndex].isBomb) {
          this.tiles[bombIndex].isBomb = true;
          numBombs += 1;
        }
      }

      // Populate numAdjacentBombs prop
      for (let k = 0; k < this.tiles.length; k++) {
        if (!this.tiles[k].isBomb) {
          let x = this.getX(k);
          let y = this.getY(k);
          let sum = 0;
          for (let i = -1; i < 2; i++) {
            for (let j = -1; j < 2; j++) {
              if (0 <= x + i && x + i < this.xLen && 0 <= y+j && y+j < this.yLen){
                let checkIndex = this.getIndex(x+i, y+j);
                if (0 <= checkIndex && checkIndex < this.tiles.length && this.tiles[checkIndex].isBomb){
                  sum += 1;
                }
              }
            }
          }
          if (sum === 0) {
            this.tiles[k].numAdjacentBombs = null;
          } else {
            this.tiles[k].numAdjacentBombs = sum;
          }
        }
      }
      this.uncoverTiles(tilexy);
    },
    uncoverTiles(tilexy) {
      let checkme = new Array();
      let checkIndex = this.getIndex(tilexy.x, tilexy.y);
      if (this.tiles[checkIndex].isBomb) {
        alert("you lost")
        return;
      }
      if (!this.tiles[checkIndex].isCleared){
        this.tiles[checkIndex].isCleared = true;
        if (!this.tiles[checkIndex].isBomb && this.tiles[checkIndex].numAdjacentBombs === null) {
          checkme.push([this.getX(checkIndex), this.getY(checkIndex)]);
        }
      }


      while (checkme.length > 0) {

        let curTile = checkme.shift()
        let x = curTile[0];
        let y = curTile[1];
        for (let i = -1; i < 2; i++) {
          for (let j = -1; j < 2; j++) {
            if (0 <= x+i && x+i < this.xLen && 0 <= y+j && y+j < this.yLen){
              let checkIndex = this.getIndex(x+i, y+j);
              if (!this.tiles[checkIndex].isCleared){
                this.tiles[checkIndex].isCleared = true;
                if (this.tiles[checkIndex].numAdjacentBombs === null) {
                  checkme.push([this.getX(checkIndex), this.getY(checkIndex)]);
                }
              }
            }
          }
        }
      }
      // Give vue something to react to
      let index = this.getIndex(tilexy.x, tilexy.y);
      this.tiles[index].isCleared = true;
      this.tiles.splice(index, 1, this.tiles[index]);


      this.checkVictory();
    },
    checkVictory() {
      let uncovered = 0;
      for (let i = 0; i < this.tiles.length; i++) {
        if (this.tiles[i].isCleared && !this.tiles[i].isBomb) {
          uncovered += 1;
        }
      }
      if (uncovered === this.tiles.length - this.bombCount) {
        alert("You win!");
      }
    }
  },
  mounted() {
    EventBus.$on('setDifficulty', this.resetBoard);
    this.resetBoard("Easy");
    EventBus.$on('tileclick', this.tileClicked);
    EventBus.$on('bombCountMounted', () => {
      EventBus.$emit('setBombCount', this.bombCount);
    });
  }
}
</script>

<style media="screen">
.game-grid {
  display: grid;
  width: 95vw;
  align-self: center;
}

@media (min-width: 800px) {
  .game-grid {
    width: 800px;
  }
}
</style>
