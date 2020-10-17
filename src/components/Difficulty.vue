<template>
  <div class="">
    <select name="difficulty" id="difficulty" v-model="difficulty" v-on:change="chgDifficulty()">
      <option value="Easy">Easy</option>
      <option value="Medium">Medium</option>
    </select>
  </div>
</template>

<script>
import EventBus from '@/store/EventBus.js';

export default {
  name: 'Difficulty',
  components: {
  },
  data () {
    return {
      difficulty: 'Easy'
    }
  },
  methods: {
    chgDifficulty() {
      EventBus.$emit('setDifficulty', this.difficulty);
    }
  },
  mounted() {
    EventBus.$on('bombCountMounted', this.chgDifficulty);
  }
}
</script>

<style scoped>
select {
  padding: 0.25em;
  border-radius: 0.5em;
  background-color: white;
}
</style>
