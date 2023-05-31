<template>
   <tarea />
</template>

<script>
import tarea from './components/Tarea'

export default {
  name: 'App',
  components: {
     tarea
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
</style>
