<template>
  <div id="app">
    <h1>Tabs application</h1>
    <hr>
    <Tabs class="tabs"/>
  </div>
</template>

<script>
import Tabs from '@/components/Tabs.vue'
export default {
  components: {
    Tabs
  }
}
</script>
<style>
h1 {
  margin-bottom: 1rem;
}
.tabs {
  margin-top: 1rem;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
