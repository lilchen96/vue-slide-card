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
  name: "test",
  props: {
    options: {
      type: Object,
    },
  },
};
</script>

<style lang="scss" scoped>
.vue-slide-card {
  display: flex;
}
</style>
