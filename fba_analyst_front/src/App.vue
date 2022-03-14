<template>
   <AllTransactions/>
</template>

<script>
import AllTransactions from './components/AllTransactions.vue'

export default {
  name: 'App',
  components: {
    AllTransactions
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
  margin-top: 60px;
}
</style>
