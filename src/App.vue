<template>
  <DownloadComponent />
</template>

<script>
import DownloadComponent from "./components/DownloadComponent.vue";

export default {
  name: "App",
  components: {
    DownloadComponent,
  },
};
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
