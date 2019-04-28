<template>
  <div id="app">
    <Manager/>
  </div>
</template>

<script>
import Manager from '@/components/Manager'

export default {
  name: 'app',
  components: {
    Manager
  }
}
</script>

<style lang="scss">

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
}

h2{
  margin: 0;
}
</style>
