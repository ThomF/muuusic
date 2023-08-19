<template>
    <div class="container">
      <div class="row">
        <div class="col-4">
          <form @submit.prevent="loadAudio">
            <select v-model="selectedAudio" @change="updateAudioUrl">
              <option value="">Select an MP3</option>
              <option v-for="mp3 in mp3Options" :key="mp3.value" :value="mp3.value">{{ mp3.label }}</option>
            </select>
            <button class="btn btn-success" type="submit" :disabled="selectedAudio === ''"><i class="mdi mdi-music"></i></button>
          </form>
        </div>
        <div class="col-4">
          <div class="music-player">
            <audio ref="audio" controls></audio>
            <AudioVisualizer :audioContext="audioContext" :sourceNode="sourceNode" />
          </div>
        </div>
      </div>
      <div id="youtube-player"></div>
    </div>
  </template>
  
  <script>
  import { ref } from "vue";
  import AudioVisualizer from "../components/AudioVisualizer.vue";
  
//   const audioFile = ('@/assets/your-audio-file.mp3'); // Import the MP3 file
  
  export default {
    components: {
      AudioVisualizer,
    },
    data() {
      return {
        selectedAudio: "", // Selected audio file
        audioUrl: "", // No need to initialize with a URL
        audioContext: null,
        sourceNode: null,
        userInteracted: false,
        mp3Options: [ // List of MP3 options
          { label: "DaftFrancis", value: "../src/assets/img/Daft Punk - Harder, Better, Faster, Stronger (Dillon Francis Remix).mp3" },
          // Add more options as needed
        ],
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
  
        // Set the source to the selected audio file
        this.$refs.audio.src = this.selectedAudio;
  
        // Create the source node and connect it to the audio context
        this.sourceNode = this.audioContext.createMediaElementSource(this.$refs.audio);
        this.sourceNode.connect(this.audioContext.destination);
      },
      updateAudioUrl() {
        // Update the audio URL when the selected option changes
        this.audioUrl = this.selectedAudio;
      },
    },
  };
  </script>
  
  <style>
  .music-player {
    /* Style your music player here */
  }
  </style>
  