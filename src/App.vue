<template>
  <CardList />
</template>

<script>
import CardList from './components/CardList.vue';


export default {
  name: 'App',
  components: {
    CardList
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
  margin-top: 10px;
  background-color: #F1F5F8;
}

body {
  background-color: #F1F5F8;
}
</style>
