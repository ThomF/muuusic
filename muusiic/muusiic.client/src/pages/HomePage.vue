<template>
  <div id="app">
    <MusicPlayer />
  </div>
</template>

<script>
import MusicPlayer from "../components/MusicPlayer.vue";

export default {
  components: {
    MusicPlayer,
  },
};
</script>

<style scoped lang="scss">
.home {
  display: grid;
  height: 80vh;
  place-content: center;
  text-align: center;
  user-select: none;

  .home-card {
    width: 50vw;

    >img {
      height: 200px;
      max-width: 200px;
      width: 100%;
      object-fit: contain;
      object-position: center;
    }
  }
}
</style>
