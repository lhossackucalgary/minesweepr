<template>
  <div class="flags-widget">
    <img src="@/assets/flag-opt.svg" alt="Flag">
    <p>{{ flags.toString().padStart(2, '0') }}</p>
  </div>
</template>

<script>
import EventBus from '@/store/EventBus.js';

export default {
  name: 'BombCount',
  components: {

  },
  data () {
    return {
      flags: 0
    }
  },
  methods: {
    addFlag() {
      // Flag added to board, rm 1 from 'flags remaining'
      this.flags -= 1;
    },
    rmFlag() {
      // Flag removed from board, add count to 'flags remaining'
      this.flags += 1;
    },
    setFlagCount(numBombs){
      this.flags = numBombs;
    }
  },
  mounted() {
    EventBus.$on('add-flag', this.addFlag);
    EventBus.$on('rm-flag', this.rmFlag);
    EventBus.$on('setBombCount', this.setFlagCount);
    EventBus.$emit('bombCountMounted');
  }
}
</script>

<style scoped>
img {
  height: 1.5em;
}

.flags-widget {
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
}
</style>
