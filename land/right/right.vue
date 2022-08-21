<template>
  <div class="box1">
    <!-- <img src="@/assets/cockpit/public/右侧导航背景.png"
         alt=""
         class="right"> -->
    <!-- 标题盒子1 -->
    <div class="header-box"
         style="margin-top:7.5vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="right-content1">
      <!-- 圆点 -->
      <div class="status-point" />
      <!-- 标题 -->
      <span class="span1">
        <!-- 内容标题填写处 -->
        土壤检测
      </span>
    </div>
    <!-- 内容盒子1 -->
    <div class="content-box1">
      <!-- 具体内容填写处 -->

      <div class="outer-data-box">
        <div class="inner-data-box"
             v-for="(item,index) in dataList1"
             :key="index">
          <img :src="item.url"
               alt=""
               class="data-img">
          <span class="data">{{item.data}}</span>
          <span class="data-title">{{item.title}}</span>
        </div>
      </div>

      <div class="outer-data-box">
        <div class="inner-data-box"
             v-for="(item,index) in dataList2"
             :key="index">
          <img :src="item.url"
               alt=""
               class="data-img">
          <span class="data">{{item.data}}</span>
          <span class="data-title">{{item.title}}</span>
        </div>
      </div>

    </div>

  </div>
</template>

<script setup>
const dataList1 = ref([
  {
    data: '34.9℃',
    title: '温度_1',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
  {
    data: '21.9%RH',
    title: '湿度_1',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
  {
    data: '6768.0lux',
    title: '光照',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
]);

const dataList2 = ref([
  {
    data: '22.7℃',
    title: '土壤温度_1',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
  {
    data: '44.2%RH',
    title: '土壤湿度_1',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
  {
    data: '0.0℃',
    title: '土壤温度_2',
    url: new URL("../../../../assets/cockpit/greenhouse/左二数据外框.png", import.meta.url).href
  },
]);

</script>


<style lang="scss" scoped>
.right {
  // background: black;
  position: absolute;
  width: 21.5vw;
  height: 90vh;
  z-index: -1;
  // margin-right: ;
}
img {
  // 解决图片失真
  image-rendering: -moz-crisp-edges; /* Firefox */
  image-rendering: -o-crisp-edges; /* Opera */
  image-rendering: -webkit-optimize-contrast; /*Webkit (non-standard naming) */
  image-rendering: crisp-edges;
  -ms-interpolation-mode: nearest-neighbor; /* IE (non-standard property) */
}
.box1 {
  pointer-events: auto;
  // background: black;
  width: 21.5vw;
  height: 90vh;
  margin-top: 5vh;
  margin-right: 3vw;
  display: flex;
  flex-direction: column;
  .header-box {
    display: flex;
    align-items: center; //垂直
    width: 15.9vw;
    height: 4vh;
    img {
      margin-left: 2.5vw;
      position: absolute;
      width: 19vw;
      height: 4vh;
    }
    .status-point {
      position: absolute;
      width: 5px;
      height: 5px;
      background: #d5f9ff;
      border-radius: 50%;
      margin-left: 3.25vw;
    }
    span {
      line-height: 4vh;
      height: 4vh;
      margin-left: 4vw;
      width: 10vw;
      font-size: 0.83vw;
      font-family: Alibaba PuHuiTi;
      font-weight: bold;
      color: #ffffff;
    }
  }
  .content-box1 {
    // background: black;
    width: 21vw;
    height: 16.5vh;
    margin-top: 0.5vh;
    .outer-data-box {
      margin-left: 2vw;
      margin-top: 0.5vh;
      width: 18.2vw;
      display: flex;
      .inner-data-box {
        margin-left: 0.7vw;
        height: 7vh;
        width: 5.6vw;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        .data-img {
          width: 5.6vw;
          height: 7vh;
        }
        .data {
          // margin-left: -4vw;
          text-align: center;
          margin-top: -6vh;
          width: 4.3vw;
          height: 1.5vh;
          font-size: 1vw;
          font-family: DINPro;
          font-weight: bold;
          color: #ffffff;
        }
        .data-title {
          text-align: center;
          margin-top: 2vh;
          width: 5vw;
          height: 1.5vh;
          font-size: 0.9vw;
          font-family: Microsoft YaHei;
          font-weight: 400;
          color: #c3f4fe;
          line-height: 1.5vh;
        }
      }
    }

    // 具体内容里的其他文字(非数据)
    span {
      font-size: 0.83vw;
      font-family: Microsoft YaHei;
      font-weight: 400;
      color: #88c4f3;
    }
  }
}
</style>
