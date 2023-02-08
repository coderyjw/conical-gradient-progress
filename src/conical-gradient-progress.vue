<template>
  <div
    class="conical-gradient-progress"
    :style="{ background: bgColor, width: size + 'px', height: size + 'px' }"
  ></div>
</template>

<script>
export default /*#__PURE__*/ {
  name: "ConicalGradientProgress", // vue component name
  props: {
    size: {
      type: Number,
      default: 400,
    },
    percentage: {
      type: Number,
      default: 0,
    },
    width: {
      type: Number,
      default: 100
    },
    strokeColor: {
      type: String | Object,
      // default: "#5a9cf8",
      default: () => ({
        "0%": "#108ee9",
        "100%": "#87d068",
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
  },
  methods: {},
};
</script>

<style scoped>
.conical-gradient-progress {
  border-radius: 50%;
  mask: radial-gradient(transparent 110px, #000 110px);
}
</style>
