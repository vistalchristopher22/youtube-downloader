<template>
  <div class="centered-container">
    <img src="@/assets/logo.png" width="250" alt="" />
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

.title {
  text-transform: uppercase;
  letter-spacing: 1.1px;
  text-shadow: 2px 2px 4px rgba(255, 255, 255, 0.5);
}

.centered-container {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  overflow: hidden;
  position: relative;
}
</style>
