<template>
  <div class="container">
    <p>
      <strong>TOP SPONSORS</strong>
    </p>
    <div class="sponsors">
      <a
        href="https://www.youtube.com/watch?v=astISOttCQ0&ab_channel=icanrockyourworld"
        target="_blank"
      >
        <img src="@/assets/gam.jpg" width="100" alt="" />
      </a>
      <a href="https://www.facebook.com/enigma.gloo" target="_blank">
        <img src="@/assets/jun.jpg" width="100" alt="" />
      </a>
      <a href="https://www.facebook.com/libudangvlog" target="_blank">
        <img src="@/assets/darem.jpg" width="100" alt="" />
      </a>
      <a href="https://www.facebook.com/ZAkventureTV" target="_blank">
        <img src="@/assets/zak.jpg" width="100" alt="" />
      </a>
    </div>
    <p></p>
    <div class="input">
      <input
        type="text"
        placeholder="Paste YouTube link"
        v-model="download.link"
        class="input-field"
      />
      <button @click="submitLink" class="start-button">Start Download</button>
    </div>
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
    <div class="progress">
      <progress :value="download.percentage" max="100"></progress>
      <p>
        <strong> {{ download.percentage }}%</strong>
      </p>
    </div>
    <span>
      <small>Code with heart by <strong>CHRis</strong> </small>
    </span>
  </div>
</template>

<style scoped>
.container {
  width: 90%;
  max-width: 600px;
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

.input-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 20px;
}

.input-field {
  width: 100%;
  max-width: 400px;
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

.start-button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 10px;
}

.start-button:hover {
  background-color: #0056b3;
}

.radio-buttons {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

label {
  margin-right: 10px;
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

@media (min-width: 768px) {
  .input-container {
    flex-wrap: nowrap;
  }
  .start-button {
    margin-left: 20px;
  }
}

.sponsors {
  display: flex;
  align-items: center;
  justify-content: space-around;
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
