<template>
  <div class="centered-container">
    <DownloadComponent />
  </div>
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
* {
  font-family: "Inter", sans-serif;
}

.centered-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  overflow: hidden;
  position: relative;
}
</style>
