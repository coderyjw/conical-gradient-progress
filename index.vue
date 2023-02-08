<template>
  <div
    class="conical-gradient-progress"
    :style="{
      background: bgColor,
      width: size + 'px',
      height: size + 'px',
      '-webkit-mask': mask,
      mask: mask,
      'clip-path': clipPath,
    }"
  ></div>
</template>

<script>
export default /*#__PURE__*/ {
  name: "ConicalGradientProgress", // vue component name
  props: {
    size: {
      type: Number,
      default: 200,
    },
    // percentage: {
    //   type: Number,
    //   default: 100,
    // },
    width: {
      type: Number,
      default: 20,
    },
    strokeColor: {
      type: String | Object,
      // default: "#5a9cf8",
      default: () => ({
        "0%": "yellow",
        "50%": "red",
        "100%": "blue",
      }),
    },
  },
  data() {
    return {
      percentage: 0,
    };
  },
  created() {
    const timer = setInterval(() => {
      if (this.percentage >= 100) {
        clearInterval(timer);
      }
      this.percentage = this.percentage + 10;
      console.log(this.percentage);
    }, 2000);
  },
  computed: {
    /* 环形进度条背景色 */
    bgColor() {
      if (typeof this.strokeColor === "object") {
        const result = Object.entries(this.strokeColor)
          .map((item) => {
            item[0] = parseFloat(item[0].slice(0, -1));
            return item;
          })
          .sort((prev, next) => prev[0] - next[0]);

        let bgColor = `conic-gradient(`;
        result.map((item, index) => {
          if (index != 0) {
            bgColor += ",";
          }
          bgColor += `${item[1]} ${(item[0] / 100) * 360}deg`;
        });
        bgColor += `)`;
        return bgColor;
      }

      return this.strokeColor;
    },
    mask() {
      const width = parseFloat(this.size) / 2 - this.width;
      console.log(`radial-gradient(transparent ${width}px, #000 ${width}px)`);
      return `radial-gradient(transparent ${width}px, #000 ${width}px)`;
    },
    clipPath() {
      let clipPath = `50% 50%, 50% 0%`;
      let degree;
      let r = this.size / 2;
      // let clipPath = ['50% 50%', '50% 0%']
      if (this.percentage <= 12.5) {
        degree = (this.percentage * 3.6 * Math.PI) / 180;
        clipPath += `, ${Math.round(Math.tan(degree) * r) + r}px 0`;
      } else if (this.percentage <= 25) {
        degree = ((this.percentage - 12.5) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0`;
        clipPath += `, 200px ${Math.round(Math.tan(degree) * r)}px`;
      } else if (this.percentage <= 37.5) {
        degree = ((this.percentage - 25) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px`;
        clipPath += `, 200px ${Math.round(Math.tan(degree) * r) + r}px`;
      } else if (this.percentage <= 50) {
        degree = ((this.percentage - 37.5) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px,200px 200px`;
        clipPath += `,${2 * r - Math.round(Math.tan(degree) * r)}px 200px`;
      } else if (this.percentage <= 62.5) {
        degree = ((this.percentage - 50) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px,200px 200px,100px 200px`;
        clipPath += `,${r - Math.round(Math.tan(degree) * r)}px 200px`;
      } else if (this.percentage <= 75) {
        degree = ((this.percentage - 62.5) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px,200px 200px,100px 200px,0px 200px`;
        clipPath += `,0 ${2 * r - Math.round(Math.tan(degree) * r)}px`;
      } else if (this.percentage <= 87.5) {
        degree = ((this.percentage - 75) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px,200px 200px,100px 200px,0px 200px,0 100px`;
        clipPath += `,0 ${r - Math.round(Math.tan(degree) * r)}px`;
      } else if (this.percentage <= 100) {
        degree = ((this.percentage - 87.5) * 3.6 * Math.PI) / 180;
        clipPath += `, 200px 0,200px 100px,200px 200px,100px 200px,0px 200px,0 100px,0 0px`;
        clipPath += `,${Math.round(Math.tan(degree) * r)}px 0%`;
      }
      console.log({ clipPath });

      return `polygon(${clipPath})`;
    },
  },
  methods: {},
};
</script>

<style scoped>
.conical-gradient-progress {
  border-radius: 50%;
  transition: clip-path 0.2s;
  /* clip-path: polygon(0 0, 50% 0, 50% 50%); */
  /* clip-path: polygon(50% 0, 100% 0, 100% 100%, 0 100%, 0 0, 50% 50%); */
  /* clip-path: polygon(50% 0, 100% 0, 100% 100%, 0 100%, 0 50%, 50% 50%); */
  /* clip-path: polygon(50% 0, 50% 50%, 100% 0); */
  /* clip-path: polygon(50% 0, 50% 0, 50% 0, 50% 0, 50% 0, 50% 50%); */
}
</style>
