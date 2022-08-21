<template>
  <div class="box1">
    <img src="@/assets/cockpit/public/右侧导航背景.png"
         alt=""
         class="right">
    <div v-for="(item,index) in list"
         :key="index">
      <div class="header-box"
           :class="headerStyle[index]">
        <img :src="item.headerBackImg"
             alt="">
        <div class="status-point" />
        <span class="span1">
          {{item.headerTitle}}
        </span>
      </div>
      <div class="content-box">
        <ul v-for="(item1,index) in item.imgList"
            :key="index">
          <li>
            <img :src="item1.url"
                 alt="">
          </li>
        </ul>
      </div>
    </div>

  </div>
</template>

<script setup>

const list = ref([
  {
    headerTitle: '特色产业',
    headerBackImg: new URL("../../../../assets/cockpit/public/具体内容标题背景.png", import.meta.url).href,
    imgList: [{ url: new URL("../../../../assets/cockpit/示例图片/3.jpg", import.meta.url).href },
    { url: new URL("../assets/cockpit/示例图片/1.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/2.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/3.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/1.jpg", import.meta.url).href },]
  },
  {
    headerTitle: '人文风景',
    headerBackImg: new URL("../../../../assets/cockpit/public/具体内容标题背景.png", import.meta.url).href,
    imgList: [{ url: new URL("../../../../assets/cockpit/示例图片/9.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/7.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/8.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/9.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/7.jpg", import.meta.url).href },]
  },
  {
    headerTitle: '生态环境',
    headerBackImg: new URL("../../../../assets/cockpit/public/具体内容标题背景.png", import.meta.url).href,
    imgList: [{ url: new URL("../../../../assets/cockpit/示例图片/6.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/4.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/5.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/6.jpg", import.meta.url).href },
    { url: new URL("../../../../assets/cockpit/示例图片/4.jpg", import.meta.url).href },]
  },
])

const headerStyle = ['headerStyle1', 'headerStyle2', 'headerStyle3']
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
  // background: black;
  pointer-events: auto;
  width: 21.5vw;
  height: 90vh;
  margin-top: 5vh;
  margin-right: 3vw;
  display: flex;
  flex-direction: column;
  .headerStyle1 {
    margin-top: 7.5vh;
  }
  .header-box {
    display: flex;
    align-items: center; //垂直
    height: 4vh;
    width: 15.9vw;
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
      margin-left: 4vw;
      height: 4vh;
      line-height: 4vh;
      width: 10vw;
      font-size: 0.83vw;
      font-family: Alibaba PuHuiTi;
      font-weight: bold;
      color: #ffffff;
    }
  }
  .content-box {
    width: 18.2vw;
    height: 18.8vh;
    position: relative;
    overflow: hidden;
    float: left;
    margin: 2.5vh 0 2.5vh 2.5vw;
    border-radius: 15px;
    span {
      font-size: 0.83vw;
      font-family: Microsoft YaHei;
      font-weight: 400;
      color: #88c4f3;
    }
    ul {
      width: 91vw;
      // height: 18.8vh;
      position: relative;
      list-style: none;
      animation: switch 10s infinite;
      padding: 0;
      margin: 0;
      li {
        width: 18.2vw;
        height: 18.8vh;
        list-style: none;
        padding: 0;
        margin: 0;
        float: left;
        img {
          height: 100%;
          width: 100%;
          border-radius: 15px;
          margin: 0;
          cursor: pointer;
          border: #68a6e0 2px solid;
        }
      }
    }
    ul:hover {
      animation-play-state: paused;
    }
  }

  @keyframes switch {
    0% {
      margin-left: -18.2vw;
      opacity: 1;
    }
    19% {
      margin-left: -18.2vw;
      opacity: 1;
    }
    26% {
      opacity: 0.5;
    }
    33% {
      margin-left: -36.4vw;
      opacity: 1;
    }
    53% {
      margin-left: -36.4vw;
      opacity: 1;
    }
    60% {
      opacity: 0.5;
    }
    67% {
      margin-left: -54.6vw;
      opacity: 1;
    }
    86% {
      margin-left: -54.6vw;
      opacity: 1;
    }
    93% {
      opacity: 0.5;
    }

    100% {
      margin-left: -72.8vw;
      opacity: 1;
    }
  }
}
</style>
