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

body {
  margin: 0;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h2{
  margin: 0;
}

ul, li {
  margin: 0;
  padding: 0;
}

li {
  list-style: none;
}
</style>
