<template>
  <div id="app">
    <div class="scoreBoard col-sm-4 offset-sm-4" >
      <Scoreboard/>
    </div>
    
  </div>
</template>

<script>
import Scoreboard from './components/Scoreboard.vue'

export default {
  name: 'app',
  components: {
    Scoreboard
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
