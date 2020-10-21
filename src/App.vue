<template>
  <div id="app">
    <Header></Header>
    <Gameboard></Gameboard>
    <LeaderBoard></LeaderBoard>
  </div>
</template>

<script>
import Gameboard from './components/Gameboard.vue';
import Header from './components/Header.vue';
import LeaderBoard from './components/LeaderBoard.vue';

export default {
  name: 'App',
  components: {
    Header,
    Gameboard,
    LeaderBoard
  }
}
</script>

<style scoped>
#app {
  display: flex;
  flex-direction: column;
  background-color: rgb(87,138,52);
}
</style>

<!-- Global scope -->
<style>
* {
   margin: 0;
   padding: 0;
}
#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  min-height: 100vh;
}

</style>
