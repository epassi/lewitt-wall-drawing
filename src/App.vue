<template>
  <div id="app">
    <wall-drawing :cell-size="0.05" :canvas-size="canvasSize" />
  </div>
</template>

<script>
import WallDrawing from "@/components/wall-drawing.vue";

export default {
  name: "app",

  components: {
    WallDrawing
  },

  data() {
    return {
      canvasSize: 400
    };
  },

  methods: {
    resizeWall() {
      this.canvasSize =
        window.innerWidth / window.innerHeight >= 1
          ? window.innerHeight
          : window.innerWidth;
    }
  },

  mounted() {
    window.onresize = this.resizeWall;
    this.resizeWall();
  }
};
</script>

<style lang="scss">
#app {
  font-family: sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  display: flex;
  align-items: center;
  justify-content: center;

  width: 100vw;
  height: 100vh;

  background: #000;
}
</style>
