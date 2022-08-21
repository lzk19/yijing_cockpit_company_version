<template>

  <div class="box1">
    <!-- <img src="@/assets/cockpit/public/左侧导航背景.png"
         alt=""
         class="left"> -->
    <div class="title-box">
      <span>
        <!-- 大标题填写处 -->
        土地管理
      </span>
      <img src="src\assets\cockpit\public\椭圆 1936.png"
           alt=""
           class="left-ellipse">

      <!-- 内容盒子1 -->
      <div class="content-box1">
        <!-- 具体内容填写处 -->
        <div class="legend-box">
          <span class="title">图例</span>
          <div class="legend-content-box">
            <div class="legend-content">
              <img src="src\assets\cockpit\land\橙色图例.png"
                   alt="">
              <span>用途1</span>
            </div>
            <div class="legend-content">
              <img src="@/assets/cockpit/land/蓝色图例.png"
                   alt="">
              <span>用途2</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
</script>

<style lang="scss" scoped>
.left {
  position: absolute;
  z-index: -1;
  width: 21.5vw;
  height: 90vh;
  margin-left: -0.5vw;
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
  width: 21.5vw;
  height: 90vh;
  margin-left: 3.5vw;
  margin-top: 5vh;
  display: flex;
  flex-direction: column;
  .title-box {
    // margin-left: 0.5vw;
    margin-top: 4vh;
    width: 7vw;
    margin-bottom: 1vh;
    span {
      font-size: 0.83vw;
      font-family: SourceHanSansCN;
      font-weight: bold;
      color: #0199e8;
      margin-left: 1.75vw;
    }
    img {
      margin-bottom: 1vh;
      width: 7vw;
    }
  }
  .header-box {
    margin-top: -1vh;
    margin-left: 0.5vw;
    height: 4vh;
    display: flex;
    align-items: center; //垂直
    width: 21.9vw;
    img {
      position: absolute;
      width: 19vw;
      height: 4vh;
      z-index: -1;
    }
    .status-point {
      position: absolute;
      width: 5px;
      height: 5px;
      background: #d5f9ff;
      border-radius: 50%;
      margin-left: 0.75vw;
    }
    span {
      margin-left: 1.5vw;
      line-height: 4vh;
      height: 4vh;
      width: 10vw;
      font-size: 0.83vw;
      font-family: Alibaba PuHuiTi;
      font-weight: bold;
      color: #ffffff;
    }
  }
  .content-box1 {
    width: 18.2vw;
    height: 18.8vh;
    margin-top: 1.5vh;
    .legend-box {
      margin-left: 1vw;
      .title {
        font-size: 1vw;
        font-family: Microsoft YaHei;
        font-weight: 400;
        color: #c3f4fe;
        margin-left: 0vw;
      }
      .legend-content-box {
        width: 5.7vw;
        height: 7vh;
        background: rgba(2, 176, 255, 0.1);
        border: 1px solid #11abee;
        margin-top: 0.5vh;
        .legend-content {
          height: 50%;
          width: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
          text-align: center;
          img {
            width: 0.4vw;
            height: 2vh;
            margin-right: -1vw;
            margin-left: -1vw;
            margin-top: 0.7vh;
          }
          span {
            font-family: SourceHanSansCN;
            font-weight: 400;
            color: #88c4f3;
            font-size: 0.9vw;
          }
        }
      }
    }
  }
}
</style>
