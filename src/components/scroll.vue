<template>
  <div class="wrapper">
    <slot></slot>
  </div>
</template>

<script>
import BScroll from "better-scroll";
export default {
  props: {
    probeType: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      scroll: null,
    };
  },
  mounted() {
    this.scroll = new BScroll(".wrapper", {
      scrollY: true,
      click: true,
      probeType: this.probeType,
    });
    //滚动时能够显示坐标，把position返回给父组件
    this.scroll.on("scroll", (position) => {
      this.$emit("scroll", position);
    });
  },
  methods: {
    refresh() {
      this.scroll.refresh();
    },
  },
};
</script>

<style>
</style>