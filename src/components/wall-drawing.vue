<template>
  <canvas
    class="wall-drawing"
    ref="wallDrawing"
    :width="canvasSize * devicePixelRatio"
    :height="canvasSize * devicePixelRatio"
    :style="{
      width: `${canvasSize}px`,
      height: `${canvasSize}px`
    }"
  />
</template>

<script>
const LINE_COUNT = 6;

export default {
  name: "WallDrawing",

  props: {
    cellSize: {
      type: Number,
      required: false,
      default: 0.1
    },
    canvasSize: {
      type: Number,
      required: false,
      default: 400
    }
  },

  data() {
    return {
      devicePixelRatio: window.devicePixelRatio,
      randomPointHistory: []
    };
  },

  computed: {},

  methods: {
    paint() {
      this.$refs.wallDrawing;
      const ctx = this.$refs.wallDrawing.getContext("2d");

      ctx.scale(window.devicePixelRatio, window.devicePixelRatio);

      ctx.strokeStyle = "#000";
      ctx.lineWidth = 0.125;

      // Grid: horizontal lines.
      for (
        let i = 0;
        i <= this.canvasSize;
        i += this.canvasSize * this.cellSize
      ) {
        ctx.beginPath();
        ctx.moveTo(0, i);
        ctx.lineTo(this.canvasSize, i);
        ctx.stroke();
      }

      // Grid: vertical lines.
      for (
        let j = 0;
        j <= this.canvasSize;
        j += this.canvasSize * this.cellSize
      ) {
        ctx.beginPath();
        ctx.moveTo(j, 0);
        ctx.lineTo(j, this.canvasSize);
        ctx.stroke();
      }

      // Red lines.
      ctx.strokeStyle = "#E0929E";
      ctx.lineWidth = 1;
      for (let k = 0; k < 4; k++) {
        const start = { x: 0, y: 0 };
        switch (k) {
          case 0:
            start.x = this.canvasSize / 2;
            start.y = 0;
            break;
          case 1:
            start.x = this.canvasSize;
            start.y = this.canvasSize / 2;
            break;
          case 2:
            start.x = this.canvasSize / 2;
            start.y = this.canvasSize;
            break;
          case 3:
            start.x = 0;
            start.y = this.canvasSize / 2;
            break;
        }
        for (let m = 0; m < LINE_COUNT; m++) {
          ctx.beginPath();
          ctx.moveTo(start.x, start.y);
          const { x, y } = this.randomPoint();
          ctx.lineTo(x, y);
          ctx.stroke();
        }
      }

      // Blue lines.
      ctx.strokeStyle = "#719AC1";
      ctx.lineWidth = 1;
      for (let k = 0; k < 4; k++) {
        const start = { x: 0, y: 0 };
        switch (k) {
          case 0:
            start.x = 0;
            start.y = 0;
            break;
          case 1:
            start.x = this.canvasSize;
            start.y = 0;
            break;
          case 2:
            start.x = this.canvasSize;
            start.y = this.canvasSize;
            break;
          case 3:
            start.x = 0;
            start.y = this.canvasSize;
            break;
        }
        for (let m = 0; m < LINE_COUNT; m++) {
          ctx.beginPath();
          ctx.moveTo(start.x, start.y);
          const { x, y } = this.randomPoint();
          ctx.lineTo(x, y);
          ctx.stroke();
        }
      }

      // Yellow lines.
      ctx.strokeStyle = "#FFD900";
      ctx.lineWidth = 1;
      for (let m = 0; m < LINE_COUNT; m++) {
        ctx.beginPath();
        ctx.moveTo(this.canvasSize / 2, this.canvasSize / 2);
        const { x, y } = this.randomPoint();
        ctx.lineTo(x, y);
        ctx.stroke();
      }
    },

    randomPoint() {
      // -2 to prevent ending at an edge.
      // (-1 to account for increment starting from +1,
      // and another -1 to avoid ending on edge).
      const maxIncrement = 1 / this.cellSize - 8;

      // +1 to prevent starting at an edge.
      const point = {
        x:
          this.canvasSize *
          this.cellSize *
          (Math.round(Math.random() * maxIncrement) + 4),
        y:
          this.canvasSize *
          this.cellSize *
          (Math.round(Math.random() * maxIncrement) + 4)
      };
      // console.log(point);

      // const pointIsDupe = this.randomPointHistory.some(
      //   item => item.x === point.x && item.y === point.y
      // );
      // if (pointIsDupe) {
      //   console.log("dupe");
      //   return this.randomPoint();
      // } else {
      this.randomPointHistory.push(point);
      return point;
      // }
    }
  },

  mounted() {
    this.paint();
  },

  updated() {
    this.paint();
  }
};
</script>

<style lang="scss" scoped>
.wall-drawing {
  background: #fff;
}
</style>
