<template>
  <div id="app">
    <h1>What are you working on today?</h1>
    <ToDo :key="renders" @re-render="rerender" />
  </div>
</template>

<script>
import ToDo from './components/ToDo.vue';

export default {
  name: 'App',
  components: {
    ToDo
  },
  data: function() {
    return {
      renders: 0
    }

  },
  methods: {
    rerender: function () {
      this.renders++;
    }
  }
}
</script>

<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  max-width: 800px;
  margin: 60px auto;
}

h1 {
  text-align: center;
}
</style>
