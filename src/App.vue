<template>
  <div id="app">
    <TicTacToe :key="key" @forceRender="restart"></TicTacToe>
  </div>
</template>

<script>
  import TicTacToe from "@/components/TicTacToe";
  export default {
    name: 'App',
    components: {
      TicTacToe
    },
    data () {
      return {
        key: 0
      }
    },
    methods: {
      restart () {
        this.key = !this.key
      }
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
*, *::before, *::after {
  box-sizing: border-box;
}
body {
  margin: 0;
}

</style>
