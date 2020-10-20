<template>
  <div class="mainbox">
    <tabbar ref="tabbar1" v-show="isShow" class="tabbar1"> </tabbar>
    <scroll
      class="wrapper"
      ref="wrapper"
      @scroll="contentscroll"
      :probeType="3"
    >
      <ul>
        <div class="box1" ref="box1">
          <ul>
            <li v-for="(item, index) in lists" :key="index">{{ item.name }}</li>
          </ul>
        </div>
        <tabbar ref="tabbar2"> </tabbar>
        <li v-for="(item, index) in lists" :key="index">{{ item.name }}</li>
      </ul>
    </scroll>
  </div>
</template>

<script>
import scroll from "./components/scroll";
import axios from "axios";
import tabbar from "./components/tabbar";
export default {
  name: "App",
  data() {
    return {
      lists: null,
      isShow: false,
      offsetTop: 0,
    };
  },
  watch: {
    lists(val, oldVal) {
      this.$nextTick(function () {
        this.offsetTop = this.$refs.tabbar2.$el.offsetTop;
      });
    },
  },
  mounted() {
    this.lists = this.getList();
  },
  methods: {
    async getList() {
      const { data: res } = await axios.get("http://localhost:9999/");
      this.lists = res.data;
      // Scroll内部用watch来侦测lists变化，一旦变化，延时20ms刷新dom，否则通过下面的钩子需要手动刷新
      this.$nextTick(function () {
        this.$refs.wrapper.refresh();
      });
    },
    //获得position
    contentscroll(position) {
      // console.log(position.y);
      this.isShow = position.y < -this.offsetTop;
    },
  },
  components: {
    scroll,
    tabbar,
  },
};
</script>

<style lang='less'>
* {
  margin: 0;
  padding: 0;
}
.mainbox {
  position: relative;
  height: 667px;
  width: 375px;
  // background: yellow;

  .tabbar1 {
    position: absolute;
    width: 100%;
    z-index: 1;
  }

  .wrapper {
    position: relative;
    text-align: center;
    height: 100%;
    width: 100%;
    background: pink;
    overflow: hidden;
    .box1 {
      width: 100%;
      background: pink;
    }
    .box2 {
      height: 100px;
      width: 100%;
      background: lightblue;
    }
  }
}

.box3 {
  position: fixed;
  top: 0px;
  z-index: 1;
  height: 100px;
  width: 100%;
  background: lightblue;
}
</style>
