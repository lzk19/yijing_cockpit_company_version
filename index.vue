<template>
  <div id="cesiumContainer"></div>
  <div id="body1">
    <Home v-if="menuActive === '概况'"></Home>
    <Greenhouse v-if="menuActive === '智慧大棚'"></Greenhouse>
    <Agriculture v-if="menuActive === '农业产业'"></Agriculture>
    <Land v-if="menuActive === '土地管理'"></Land>
    <el-footer>
      <img src="@/assets/cockpit/public/底部框架.png"
           class="bottom-img"
           alt="">
      <div class="btn-box-box">
        <div class="btn-box"
             v-for="(item,index) in menuList"
             @click="menuClick(item)"
             :key="index">
          <img :src="menuActive===item.title?item.url2:item.url"
               class="bottom-left">
          <span class="title">{{ item.title }}</span>
        </div>
      </div>
    </el-footer>
  </div>
</template>

<script setup>
import Home from './home/home.vue'
import Greenhouse from './greenhouse/greenhouse.vue'
import Agriculture from './agriculture/agriculture.vue'
import Land from './land/land.vue'
// import BigScreenMap from "@/Gis/BigScreenMap";
import { provide, ref } from "vue";

let bigScreenMap = ref(null);
provide('bigScreenMap', bigScreenMap);

// onMounted(() => {
//   bigScreenMap.value = new BigScreenMap("cesiumContainer");
// });

const menuActive = ref('概况');
const menuList = ref([
  {title: '概况'},
  {title: '智慧大棚'},
  {title: '农业产业'},
  {title: '土地管理'},
  // { title: '概况', url: new URL("../assets/cockpit/public/底部左1按钮未点.png", import.meta.url).href, url2: new URL("../assets/cockpit/public/底部左1按钮已点.png", import.meta.url).href },
  // { title: '智慧大棚', url: new URL("../../assets/cockpit/public/底部中按钮未点.png", import.meta.url).href, url2: new URL("../../assets/cockpit/public/底部中按钮已点.png", import.meta.url).href },
  // { title: '农业产业', url: new URL("../../assets/cockpit/public/底部中按钮未点.png", import.meta.url).href, url2: new URL("../../assets/cockpit/public/底部中按钮已点.png", import.meta.url).href },
  // { title: '土地管理', url: new URL("../../assets/cockpit/public/底部右1按钮未点.png", import.meta.url).href, url2: new URL("../../assets/cockpit/public/底部右1按钮已点.png", import.meta.url).href },
]);

function menuClick (item) {
  menuActive.value = item.title;
}
</script>

<style lang="scss" scoped>
#body1 {
  position: absolute;
  z-index: 1;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  pointer-events: none;
}
#cesiumContainer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.el-footer {
  height: 7.5vh;
  display: flex;
  justify-content: center;
  align-items: center;
  pointer-events: auto;
  position: absolute;
  bottom: 5vh;
}
.bottom-img {
  // position: absolute;
  height: 7.5vh;
  width: 33vw;
  margin-top: 1.9vh;
  margin-left: 32.3vw;
}
.btn-box-box {
  margin-left: -28.8vw;
  display: flex;
  .btn-box {
    height: 4.1vh;
    width: 6.2vw;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 3vh;
    cursor: pointer;
    .bottom-left {
      width: 6.2vw;
      height: 3.8vh;
    }
    .title {
      line-height: 3vh;
      text-align: center;
      height: 4.1vh;
      width: 7vw;
      margin-left: -6.2vw;
      font-size: 1vw;
      font-family: Alibaba PuHuiTi;
      font-weight: 400;
      color: #ffffff;
    }
  }
}
</style>
