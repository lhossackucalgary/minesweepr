<template>
  <div class="header">
    <h1>Minesweepr</h1>
    <div class="hud">
      <Difficulty/>
      <div class="bombtimer">
        <BombCount/>
        <Timer />
      </div>
      <MiscControls />
    </div>
  </div>
</template>

<script>
import Difficulty from './Difficulty.vue';
import Timer from './Timer.vue';
import BombCount from './BombCount.vue';
import MiscControls from './MiscControls.vue';

export default {
  name: 'Header',
  components: {
    Timer,
    Difficulty,
    BombCount,
    MiscControls
  },
  data () {
    return {

    }
  }
}
</script>

<style scoped>
.header {
padding: 0.5em;
  color: white;
  background-color: rgb(74,117,44);
}

.hud {
  margin-top: 0.25em;
  display: grid;
  grid-template-columns: repeat(3, 1fr)
}

.bombtimer {
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
  column-gap: 1em;
  row-gap: 0.5em;
}

</style>

<style media="screen">
.bombtimer p {
  display: inline;
  margin-left: 0.25em;
  margin-right: 0.25em;
  font-size: 1.25em;
}
</style>
