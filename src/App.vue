<template>
  <div id="app">
    <div class="wrapper">
      <SuperTable />
      <!-- <BaseTable /> -->
    </div>
  </div>
</template>

<script>
import SuperTable from './components/super-table';
// import BaseTable from './components/base-table';

export default {
  name: 'App',
  components: {
    SuperTable,
    // BaseTable
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

* {
  box-sizing: border-box;
}
</style>

<style scope>
  .wrapper {
    width: 1300px;
    margin-left: 50px;
    text-align: left;
  }
  .control {
    display: inline-flex;
    gap: 20px;
    margin-bottom: 20px;
  }
</style>
