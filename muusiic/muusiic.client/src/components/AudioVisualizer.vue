<template>
    <div class="audio-visualizer">
      <canvas ref="canvas"></canvas>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      audioContext: Object,
      sourceNode: Object,
    },
    mounted() {
      if (this.audioContext && this.sourceNode) {
        this.setupVisualizer();
      }
    },
    methods: {
      setupVisualizer() {
        const analyser = this.audioContext.createAnalyser();
        analyser.fftSize = 256;
        const bufferLength = analyser.frequencyBinCount;
        const dataArray = new Uint8Array(bufferLength);
  
        const canvas = this.$refs.canvas;
        const canvasCtx = canvas.getContext("2d");
  
        this.sourceNode.connect(analyser);
        analyser.connect(this.audioContext.destination);
  
        const draw = () => {
          requestAnimationFrame(draw);
  
          analyser.getByteFrequencyData(dataArray);
  
          canvasCtx.clearRect(0, 0, canvas.width, canvas.height);
  
          const barWidth = (canvas.width / bufferLength) * 2;
          let x = 0;
  
          for (let i = 0; i < bufferLength; i++) {
            const barHeight = dataArray[i];
            canvasCtx.fillStyle = `rgb(${barHeight}, 0, 0)`;
            canvasCtx.fillRect(x, canvas.height - barHeight, barWidth, barHeight);
  
            x += barWidth + 1;
          }
        };
  
        draw();
      },
    },
  };
  </script>
  
  <style>
  .audio-visualizer {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 200px;
    background-color: #333;
  }
  
  canvas {
    width: 100%;
    height: 100%;
  }
  </style>
  