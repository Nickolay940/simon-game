<template>
  <div id="app">
    <h1>Simon game</h1>
    <GameField />
  </div>
</template>

<script>
import GameField from "./components/GameField.vue";

export default {
  name: "App",
  components: {
    GameField,
  },

};
</script>

<style lang="stylus">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1 {
  margin 50px 0
}

</style>
