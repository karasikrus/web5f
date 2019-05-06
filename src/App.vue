<template>
  <div id="app">
    <img alt="Vue logo" src="assets/logo.png">
    <create-document @selected-changed="onSelectedIdChanged" @refresh-list="refreshList"></create-document>


  </div>
</template>

<script>


import CreateDocument from "./components/create-document";
export default {
  name: 'app',
  components: {
    CreateDocument

  },
  data() {
    return {
      selectedId: "",
    }
  },
  methods: {
    onSelectedIdChanged(selectedId) {
      this.selectedId = selectedId;
    },
    refreshList() {
      this.$refs.list.refreshList();
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
