<template>
  <div id="app">
    <Table />
  </div>
</template>

<script>
import Table from "./components/Table.vue";
export default {
  name: "App",
  components: {
    Table,
  },
};
</script>
<style>
html {
  background: url("./assets/wp2736042-football-ground-wallpaper.jpg") no-repeat
    center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  padding: 20px;
}
</style>
