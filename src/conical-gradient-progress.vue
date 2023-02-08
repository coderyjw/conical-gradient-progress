<template>
  <div
    class="conical-gradient-progress-wrapper"
    :style="{ width: size + 'px', height: size + 'px' }"
  >
    <div
      class="progress-bg"
      :style="{ '-webkit-mask': mask, mask: mask }"
    ></div>
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
    <div
      class="inner-label"
      :style="{
        fontSize: textSize + 'px',
        width: size + 'px',
        height: size + 'px',
        lineHeight: size + 'px',
        color: fontColor,
      }"
    >
      <slot> {{ percentage }}%</slot>
    </div>
  </div>
</template>

<script>
export default /*#__PURE__*/ {
  name: "ConicalGradientProgress", // vue component name
  props: {
    textSize: {
      type: Number,
      default: 40,
    },
    fontColor: {
      type: String,
      default: "#000",
    },
    size: {
      type: Number,
      default: 200,
    },
    percentage: {
      type: Number,
      default: 70,
    },
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
    return {};
  },
  created() {},
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
      return `radial-gradient(transparent ${width}px, #000 ${width}px)`;
    },
    clipPath() {
      // let clipPath = `50% 50%, 50% 0%`;
      let degree;
      let r = this.size / 2;
      let clipPath = ["50% 50%", "50% 0%"];
      if (this.percentage <= 12.5) {
        degree = (this.percentage * 3.6 * Math.PI) / 180;
        clipPath.push(
          ...new Array(8).fill(`${Math.round(Math.tan(degree) * r) + r}px 0`)
        );
      } else if (this.percentage <= 25) {
        degree = ((this.percentage - 12.5) * 3.6 * Math.PI) / 180;
        clipPath.push("200px 0");
        clipPath.push(
          ...new Array(7).fill(`200px ${Math.round(Math.tan(degree) * r)}px`)
        );
      } else if (this.percentage <= 37.5) {
        degree = ((this.percentage - 25) * 3.6 * Math.PI) / 180;
        clipPath.push(...["200px 0", "200px 100px"]);
        clipPath.push(
          ...new Array(6).fill(
            `200px ${Math.round(Math.tan(degree) * r) + r}px`
          )
        );
      } else if (this.percentage <= 50) {
        degree = ((this.percentage - 37.5) * 3.6 * Math.PI) / 180;
        clipPath.push(...["200px 0", "200px 100px", "200px 200px"]);
        clipPath.push(
          ...new Array(5).fill(
            `${2 * r - Math.round(Math.tan(degree) * r)}px 200px`
          )
        );
      } else if (this.percentage <= 62.5) {
        degree = ((this.percentage - 50) * 3.6 * Math.PI) / 180;
        clipPath.push(
          ...["200px 0", "200px 100px", "200px 200px", "100px 200px"]
        );
        clipPath.push(
          ...new Array(4).fill(
            `${r - Math.round(Math.tan(degree) * r)}px 200px`
          )
        );
      } else if (this.percentage <= 75) {
        degree = ((this.percentage - 62.5) * 3.6 * Math.PI) / 180;
        clipPath.push(
          ...[
            "200px 0",
            "200px 100px",
            "200px 200px",
            "100px 200px",
            "0px 200px",
          ]
        );
        clipPath.push(
          ...new Array(3).fill(
            `0 ${2 * r - Math.round(Math.tan(degree) * r)}px`
          )
        );
      } else if (this.percentage <= 87.5) {
        degree = ((this.percentage - 75) * 3.6 * Math.PI) / 180;
        clipPath.push(
          ...[
            "200px 0",
            "200px 100px",
            "200px 200px",
            "100px 200px",
            "0px 200px",
            "0 100px",
          ]
        );
        clipPath.push(
          ...new Array(2).fill(`0 ${r - Math.round(Math.tan(degree) * r)}px`)
        );
      } else if (this.percentage <= 100) {
        degree = ((this.percentage - 87.5) * 3.6 * Math.PI) / 180;
        clipPath.push(
          ...[
            "200px 0",
            "200px 100px",
            "200px 200px",
            "100px 200px",
            "0px 200px",
            "0 100px",
            "0 0px",
          ]
        );
        clipPath.push(
          ...new Array(1).fill(`${Math.round(Math.tan(degree) * r)}px 0%`)
        );
      }

      return `polygon(${clipPath.join(",")})`;
    },
  },
  methods: {},
};
</script>

<style scoped>
.conical-gradient-progress-wrapper {
  position: relative;
}
.progress-bg {
  background-color: #ebeef5;
  width: 100%;
  height: 100%;
  border-radius: 50%;
}
.conical-gradient-progress {
  border-radius: 50%;
  transition: clip-path 0.2s;
  position: absolute;
  top: 0;
  left: 0;
}

.inner-label {
  position: absolute;
  top: 0;
  left: 0;

  line-height: 100%;
  text-align: center;
  position: absolute;
  pointer-events: none;
}
</style>
