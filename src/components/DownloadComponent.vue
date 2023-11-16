<template>
  <div class="container">
    <h1>YouTube Video Downloader</h1>
    <input
      type="text"
      placeholder="Paste YouTube link"
      v-model="download.link"
      class="input-field"
    />
    <div class="radio-buttons">
      <label>
        <input type="radio" value="mp3" v-model="download.type" />
        MP3
      </label>
      <label>
        <input type="radio" value="mp4" v-model="download.type" />
        MP4
      </label>
    </div>
    <button @click="submitLink" class="start-button">Start Download</button>
    <div class="progress">
      <progress :value="download.percentage" max="100"></progress>
      <p>
        <strong> {{ download.percentage }}%</strong>
      </p>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 20vw;
  margin: 0 auto;
  text-align: center;
  background-color: #f4f4f4;
  padding: 15px;
  border-radius: 10px;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.input-field {
  width: 18vw;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.radio-buttons {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

label {
  margin-right: 10px;
}

.start-button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.start-button:hover {
  background-color: #0056b3;
}

.progress {
  font-size: 18px;
  margin-top: 20px;
}

progress {
  width: 100%;
  height: 20px;
  border: none;
  background-color: #ddd;
  border-radius: 5px;
  animation: progress-bar-stripes 1s linear infinite;
}

progress::-webkit-progress-bar {
  background-color: #ddd;
  border-radius: 5px;
}

progress::-webkit-progress-value {
  background-color: #007bff;
  border-radius: 5px;
}

@keyframes progress-bar-stripes {
  from {
    background-position: 1rem 0;
  }
  to {
    background-position: 0 0;
  }
}
</style>

<script>
import { ref } from "vue";
import axios from "axios";
export default {
  setup() {
    const io = require("socket.io-client");
    const download = ref({
      link: "",
      type: "mp3",
      percentage: 0,
      code: null,
    });

    const socket = io("https://surigaodelsur.gov.ph:3000");
    socket.on("download-progress", (data) => {
      download.value.percentage = data;
    });

    const submitLink = () => {
      axios
        .post(`https://surigaodelsur.gov.ph:3000/download`, download.value)
        .then((response) => {
          if (response.status === 200) {
            download.value.code = response.data.code;
            window.location.href = `https://surigaodelsur.gov.ph:3000/download-file?code=${response.data.code}&type=${response.data.type}`;
          }
        });
    };

    return {
      download,
      submitLink,
    };
  },
};
</script>
