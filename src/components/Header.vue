<template>
  <div class="header" ref="header">
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
import Difficulty from '@/components/Difficulty.vue';
import Timer from '@/components/Timer.vue';
import BombCount from '@/components/BombCount.vue';
import MiscControls from '@/components/MiscControls.vue';

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
  display: grid;
  grid-template-columns: 1fr;
}

.hud {
  margin-top: 0.25em;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.bombtimer {
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: center;
  column-gap: 1em;
  row-gap: 0.5em;
}

@media (min-width: 800px) {
  .header {
    grid-template-columns: 1fr 800px 1fr;
  }
  .header h1 {
    grid-column-start: 2;
  }
  .hud {
    grid-column: 2/3;
    grid-row: 2/2;
  }
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
