<template>
  <div>
    <input
      type="text"
      placeholder="Enter youtube link"
      v-model="download.link"
    />
    <div>
      <input type="radio" id="mp3" value="mp3" v-model="download.type" />
      <label for="mp3">MP3</label>
    </div>
    <div>
      <input type="radio" id="mp4" value="mp4" v-model="download.type" />
      <label for="mp4">MP4</label>
    </div>
    <button @click="submitLink">Start</button>
    <div>Progress : {{ download.percentage }}</div>
  </div>
</template>

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

    const socket = io("http://51.79.242.246:3000");
    socket.on("download-progress", (data) => {
      download.value.percentage = data;
    });

    const submitLink = () => {
      axios
        .post(`http://51.79.242.246:3000/download`, download.value)
        .then((response) => {
          if (response.status === 200) {
            download.value.code = response.data.code;
            window.location.href = `http://51.79.242.246:3000/download-file?code=${response.data.code}&type=${response.data.type}`;
            console.log("display the download button");
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
