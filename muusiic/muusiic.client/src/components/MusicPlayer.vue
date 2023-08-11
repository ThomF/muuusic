<template>
    <div class="container">
      <div class="row">
        <div class="col-4">
          <form @submit.prevent="loadAudio">
            <input type="text" placeholder="MUSIC URL" v-model="audioUrl">
            <button class="btn btn-success" type="submit"><i class="mdi mdi-music"></i></button>
          </form>
        </div>
        <div class="col-4">
          <div class="music-player">
            <audio ref="audio" controls></audio>
            <AudioVisualizer :audioContext="audioContext" :sourceNode="sourceNode" />
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from "vue";
  import AudioVisualizer from "../components/AudioVisualizer.vue";
  
  export default {
    components: {
      AudioVisualizer,
    },
    data() {
      return {
        audioUrl: "",
        audioContext: null,
        sourceNode: null,
        userInteracted: false,
      };
    },
    methods: {
      loadAudio() {
        if (!this.userInteracted) {
          this.userInteracted = true;
          this.setupAudio();
        }
      },
      setupAudio() {
        // Create the audio context
        this.audioContext = new (window.AudioContext || window.webkitAudioContext)();
  
        // Load the audio
        this.$refs.audio.src = this.audioUrl;
        this.$refs.audio.addEventListener("loadedmetadata", () => {
          // Create the source node and connect it to the audio context
          this.sourceNode = this.audioContext.createMediaElementSource(this.$refs.audio);
          this.sourceNode.connect(this.audioContext.destination);
        });
      },
    },
  };
  </script>
  
  <style>
  .music-player {
    /* Style your music player here */
  }
  </style>
  