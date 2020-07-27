<template>
  <div class="vue-slide-card" :style="height:options.height">
    <div class="content">
      <slot name="content"></slot>
    </div>
    <div class="operation">
      <slot name="operation"></slot>
    </div>
  </div>
</template>

<script>
export default {
  components: {
    uniCard,
  },
  data() {
    return {
      cardWidth: "",
      cardOffset: 0,
      operationOpacity: 0,
      startPoint: {
        x: "",
        y: "",
      },
    };
  },
  props: {
    paramsVal: {
      type: Object,
      default: null,
    },
    // 是否可删除
    isDelete: {
      type: Boolean,
      default: false,
    },
    operationWidth: {
      type: Number,
      default: 50,
    },
    // 删除是否可见
    deleteVisible: {
      type: false,
      default: false,
    },
  },
  computed: {
    getCardStyle() {
      let result = `transform: translateX(${this.cardOffset}px);`;
      return result;
    },
  },
  watch: {
    deleteVisible: {
      handler(val) {
        if (val) {
          this.operationOpacity = 1;
          this.cardOffset = this.operationWidth * -1;
        } else {
          this.operationOpacity = 0;
          this.cardOffset = 0;
        }
      },
    },
  },
  methods: {
    goDetail() {
      this.$emit("goDetailFn");
    },
    // 点击删除
    handleDelete() {
      this.$emit("handleDelete");
    },
    touchstart(e) {
      if (!this.isDelete) {
        return;
      }
      if (e.changedTouches.length > 1) {
        return;
      }
      this.startPoint = {
        x: e.changedTouches[0].pageX,
        y: e.changedTouches[0].pageY,
      };
    },
    touchmove(e) {},
    touchend(e) {
      if (!this.isDelete) {
        return;
      }
      if (e.changedTouches.length > 1) {
        return;
      }
      const endPoint = {
        x: e.changedTouches[0].pageX,
        y: e.changedTouches[0].pageY,
      };
      const xOffset = endPoint.x - this.startPoint.x;
      if (Math.abs(xOffset) <= 40) {
        return;
      }
      if (xOffset > 0) {
        this.operationOpacity = 0;
        this.cardOffset = 0;
        this.$emit("update:deleteVisible", false);
      }
      if (xOffset < 0) {
        this.operationOpacity = 1;
        this.cardOffset = this.operationWidth * -1;
        this.$emit("update:deleteVisible", true);
        this.$emit("show-delete");
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.vue-slide-card {
  display: flex;
}
</style>
