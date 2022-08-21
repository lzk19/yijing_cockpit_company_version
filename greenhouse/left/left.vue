<template>

  <div class="box1">
    <img src="@/assets/cockpit/public/左侧导航背景.png"
         alt=""
         class="left">
    <div class="title-box">
      <span>
        <!-- 大标题填写处 -->
        智慧大棚
      </span>
      <img src="src\assets\cockpit\public\椭圆 1936.png"
           alt=""
           class="left-ellipse">
    </div>

    <!-- 标题盒子1 -->
    <div class="header-box">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content1">
      <!-- 圆点 -->
      <div class="status-point" />
      <!-- 标题 -->
      <span>
        <!-- 内容标题填写处 -->
        大棚产值
      </span>
      <span class="view-all"
            @click="dialogVisible = true">查看全部</span>
    </div>
    <el-dialog v-model="dialogVisible"
               width="0%">
      <Dialog @updateDialog="updateDialog"></Dialog>
    </el-dialog>
    <!-- 内容盒子1 -->
    <div class="content-box1">
      <!-- 具体内容填写处 -->
      <el-carousel trigger="click"
                   class="carousel"
                   interval="4000"
                   height="18.8vh">
        <el-carousel-item v-for="(item,index) in greenhouseList.slice(0,4)"
                          :key="index"
                          class="carousel-item"
                          v-loading="loading">
          <img :src="item.url"
               alt="">
          <div class="content-title-box">
            <img :src="titleImg"
                 alt=""
                 class="content-title">
            <span class="title">
              {{item.greenhouseName}}
            </span>
          </div>
          <div class="page-footer">
            <img :src="footerImg"
                 alt=""
                 class="footer-img">
            <span class="footer-text">
              {{index+1}} / 4
            </span>
          </div>
        </el-carousel-item>
      </el-carousel>
    </div>

    <!-- 标题盒子2 -->
    <div class="header-box"
         style="margin-top:2vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content2">
      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        大棚喷滴灌
      </span>
    </div>
    <!-- 内容盒子2 -->
    <div class="content-box2">
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

    <!-- 标题盒子3 -->
    <div class="header-box"
         style="margin-top:1.5vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content3">

      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        大棚占地面积排名
      </span>
    </div>
    <!-- 内容盒子3 -->
    <div class="content-box3">
      <!-- 具体内容填写处 -->
      <!-- 排名 -->
      <div class="area-box"
           v-for="(item,index) in greenhouseList.slice(0,5)"
           :key="index">
        <span :class="indexStyle[index]">
          {{index+1}}
        </span>
        <span class="type">
          {{item.greenhouseName}}
        </span>
        <span class="area">
          占地面积：{{item.greenhouseArea}}
        </span>
        <div class="percentage-box">
          <span class="percentage">
            {{item.greenhouseAreaPercentage}}%
          </span>
          <span class="percentage-text">
            {{percentageText}}
          </span>
        </div>
        <el-progress :text-inside="true"
                     :show-text="false"
                     :stroke-width="6"
                     :color="item.progressColor"
                     :class="progressStyle[index]"
                     :percentage="item.greenhouseAreaPercentage"
                     class="progress">
        </el-progress>
      </div>
    </div>

  </div>
</template>

<script setup>
import Dialog from '../dialog/dialog.vue'
import { listGreenhouse, getGreenhouse, delGreenhouse, addGreenhouse, updateGreenhouse } from "@/api/basics/greenhouse";
import { ref } from '@vue/reactivity';
const dialogVisible = ref(false)

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


const progressColor = ref([
  {
    color: 'red',
  }
])

const percentageText = ref("总占比率")
const indexStyle = ref(['indexStyle1', 'indexStyle2', 'indexStyle3', 'indexStyle4', 'indexStyle5'])
const progressStyle = ref(['progressStyle1', 'progressStyle2', 'progressStyle3', 'progressStyle4', 'progressStyle5'])
const titleImg = ref(new URL("../../../../assets/cockpit/greenhouse/左一内容标题.png", import.meta.url).href,)
const footerImg = ref(new URL("../../../../assets/cockpit/greenhouse/页脚.png", import.meta.url).href,)
const greenhouseList = ref([]);
const loading = ref(true);
const showSearch = ref(true);
const ids = ref([]);
const single = ref(true);
const multiple = ref(true);
const total = ref(0);
const title = ref("");

const data = reactive({
  form: {},
  queryParams: {
    greenhouseName: null,
    greenhouseArea: null,
    greenhouseUse: null,
    position: null
  },
  rules: {
    id: [
      { required: true, message: "id不能为空", trigger: "blur" }
    ],
  }
});
const { queryParams, form, rules } = toRefs(data);
let totalArea = 0
/** 查询智慧大棚管理列表 */
function getList () {
  loading.value = true;
  listGreenhouse(queryParams.value).then(response => {
    greenhouseList.value = response.rows;
    total.value = response.total;
    loading.value = false;
    greenhouseList.value.sort((a, b) => {
      return b.greenhouseArea.slice(0, -1) - a.greenhouseArea.slice(0, -1)
    })
    for (let i = 0; i < greenhouseList.value.length; i++) {
      totalArea += Number(greenhouseList.value[i].greenhouseArea.slice(0, -1))
    }
    for (let i = 0; i < greenhouseList.value.length; i++) {
      greenhouseList.value[i].greenhouseAreaPercentage = Number(greenhouseList.value[i].greenhouseArea.slice(0, -1) / totalArea * 100).toFixed(1)
    }
    greenhouseList.value[0].progressColor = "#00ff00"
    greenhouseList.value[1].progressColor = "#00BFFF"
    greenhouseList.value[2].progressColor = "#BA55D3"
    greenhouseList.value[3].progressColor = "lightgray"
    greenhouseList.value[4].progressColor = "lightgray"
  })
};
getList();

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
    .view-all {
      text-align: center;
      line-height: 2.5vh;
      // padding-top: -0.5vh;
      width: 4.5vw;
      height: 3vh;
      border: #0bb3ef 2px solid;
      border-radius: 2px;
      font-size: 0.83vw;
      font-family: Alibaba PuHuiTi;
      font-weight: bold;
      color: #27caff;
      cursor: pointer;
    }
  }
  .content-box1 {
    width: 18.2vw;
    height: 18.8vh;
    margin-top: 1.5vh;
    .content-title-box {
      position: relative;
      margin-top: -20vh;
      margin-left: -0.3vw;
      width: 5vw;
      height: 4vh;
      display: flex;
      justify-content: center;
      align-items: center;
      .content-title {
        width: 100%;
        height: 100%;
        border: none;
      }
      .title {
        position: absolute;
        text-align: center;
        width: 5vw;
        height: 2.8vh;
        line-height: 2.8vh;
        font-size: 0.75vw;
        font-family: Alibaba PuHuiTi;
        font-weight: bold;
        color: #ebf6ff;
      }
    }

    .page-footer {
      margin-top: 12.9vh;
      margin-left: 14.6vw;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      width: 2.7vw;
      height: 3vh;
      .footer-img {
        border: #27c9fe 1px solid;
        width: 2.7vw;
        height: 3vh;
      }
      .footer-text {
        text-align: center;
        margin-left: -2.7vw;
        width: 2.7vw;
        height: 3vh;
        font-size: 0.75vw;
        font-family: Alibaba PuHuiTi;
        font-weight: bold;
        color: #ebf6ff;
        line-height: 3vh;
      }
    }

    .carousel {
      border-radius: 10px;
      margin-left: 0.8vw;
      z-index: 0;
      border: #27c9fe 2px solid;
    }
    img {
      width: 100%;
      height: 100%;
      border-radius: 10px;
      cursor: pointer;
    }

    // 改变指示器为圆点
    :deep(.el-carousel__indicator--horizontal .el-carousel__button) {
      width: 10px;
      height: 10px;
      background: transparent;
      border: 1px solid #ffffff;
      border-radius: 50%;
      opacity: 0.5;
    }
    :deep(.el-carousel__indicator--horizontal.is-active .el-carousel__button) {
      width: 10px;
      height: 10px;
      background: #ffffff;
      border-radius: 50%;
      opacity: 1;
    }
  }

  .content-box2 {
    // background: black;
    width: 21vw;
    height: 16.5vh;
    margin-top: 0.5vh;
    .outer-data-box {
      // margin-left: 1vw;
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

  .content-box3 {
    width: 20vw;
    height: 26vh;
    margin-top: 1vh;
    .area-box {
      width: 20vw;
      height: 5.2vh;
      margin-top: 0.2vh;
      .indexStyle1 {
        color: red;
        font-style: italic;
        font-size: 1.1vw;
        margin-left: 0.5vw;
      }
      .indexStyle2 {
        color: #00ff00;
        font-style: italic;
        font-size: 1.1vw;
        margin-left: 0.5vw;
      }
      .indexStyle3 {
        color: #00bfff;
        font-style: italic;
        font-size: 1.1vw;
        margin-left: 0.5vw;
      }
      .indexStyle4 {
        color: lightgray;
        font-style: italic;
        font-size: 1.1vw;
        opacity: 0.2;
        margin-left: 0.5vw;
      }
      .indexStyle5 {
        color: lightgray;
        font-style: italic;
        font-size: 1.1vw;
        opacity: 0.2;
        margin-left: 0.5vw;
      }
      .type {
        margin-left: 0.5vw;
        height: 2vh;
        font-size: 1.1vw;
        font-family: SourceHanSansCN;
        font-weight: bold;
        color: #aad4ff;
        line-height: 2.5vh;
      }
      .area {
        margin-left: 1vw;
        height: 1.5vh;
        font-size: 0.73vw;
        font-family: SourceHanSansCN;
        font-weight: bold;
        color: #95afd2;
        line-height: 2.5vh;
      }
      .percentage-box {
        display: flex;
        flex-direction: column;
        margin-left: 15vw;
        margin-top: -3vh;
        .percentage {
          width: 3.4vw;
          font-size: 1.2vw;
          font-family: SourceHanSansCN;
          font-weight: bold;
          color: #0fbdff;
          line-height: 2.5vh;
        }
        .percentage-text {
          margin-top: -0.5vh;
          width: 4vw;
          font-size: 0.6vw;
          font-family: SourceHanSansCN;
          font-weight: bold;
          color: #95afd2;
          line-height: 2.5vh;
        }
      }
      .progress {
        margin-left: 0.5vw;
        width: 14vw;
        margin-top: -1.5vh;
        border: 1px solid #9999ff;
        border-radius: 10px;
      }

      .progressStyle4,
      .progressStyle5 {
        opacity: 0.2;
      }
      :deep(.el-progress-bar__outer) {
        background-color: transparent;
      }
    }
  }
}
</style>
