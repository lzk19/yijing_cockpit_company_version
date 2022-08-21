<template>
  <div class="box1">
    <img src="@/assets/cockpit/public/右侧导航背景.png"
         alt=""
         class="right">
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
        产业分类变化趋势
      </span>
    </div>
    <!-- 内容盒子1 -->
    <div class="content-box1">
      <!-- 具体内容填写处 -->
      <!-- 饼状图 -->
      <div id="content1"
           ref="content1">

      </div>

    </div>

    <!-- 标题盒子2 -->
    <div class="header-box"
         style="margin-top:-3vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="right-content2">
      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        农业历年产值
      </span>
    </div>
    <!-- 内容盒子2 -->
    <div class="content-box2">
      <!-- 具体内容填写处 -->
      <div id="content2"
           ref="content2">
      </div>
    </div>

    <!-- 标题盒子3 -->
    <div class="header-box">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="right-content3">

      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        农业历年产值
      </span>
    </div>
    <!-- 内容盒子3 -->
    <div class="content-box3">
      <!-- 具体内容填写处 -->

      <div class="outputValue-box"
           v-for="(item,index) in farmingList"
           :key="index">
        <div class="index-box">
          <img :src="item.indexBackImg"
               alt="">
          <span class="index">
            {{index+1}}
          </span>
        </div>
        <span class="name">
          {{item.farmingName}}
        </span>
        <span class="value">
          {{item.totalOutput}}
        </span>
        <el-progress :text-inside="true"
                     :stroke-width="9"
                     :percentage="item.percentage"
                     class="progress"
                     color="#39ffdc"
                     :show-text="false">
        </el-progress>
        <span class="percentage">
          {{item.percentage}}%
        </span>
        <img :src="backImg"
             alt=""
             class="img1">
      </div>

    </div>

  </div>
</template>


<script setup>
import { onMounted } from '@vue/runtime-core';
import * as echarts from 'echarts';
import { listFarming, getFarming, delFarming, addFarming, updateFarming } from "@/api/basics/farming";
import { listInfo, getInfo, delInfo, addInfo, updateInfo } from "@/api/basics/info";
const { proxy } = getCurrentInstance();

const backImg = ref(new URL("../../../../assets/cockpit/agriculture/右三排名背景.png", import.meta.url).href)
const farmingList = ref([]);
const open = ref(false);
const loading = ref(true);
const showSearch = ref(true);
const ids = ref([]);
const single = ref(true);
const multiple = ref(true);
const total = ref(0);
const title = ref("");
const content1 = ref()
const content2 = ref()
onMounted(() => {
  displayEcharts1()
  displayEcharts2()
})

const openIndustryInfo = ref();

const data = reactive({
  form: {},
  queryParams: {
    pageNum: 1,
    pageSize: 10,
    farmingName: null,
    farmingType: null,
    position: null
  },
  rules: {
    id: [
      { required: true, message: "id不能为空", trigger: "blur" }
    ],
  }
});
const { queryParams, form, rules } = toRefs(data);

const infoList = ref([]);
const openInfo = ref(false);
const loadingInfo = ref(true);
const showSearchInfo = ref(true);
const idsInfo = ref([]);
const singleInfo = ref(true);
const multipleInfo = ref(true);
const totalInfo = ref(0);
const titleInfo = ref("");

const dataInfo = reactive({
  formInfo: {},
  queryParamsInfo: {
    farmingId: null,
    productName: null,
    output: null,
    salesVolume: null,
    productYear: null,
  },
  rulesInfo: {
    id: [
      { required: true, message: "id不能为空", trigger: "blur" }
    ],
    farmingId: [
      { required: true, message: "农业产业id不能为空", trigger: "blur" }
    ],
  }
});

const { queryParamsInfo, formInfo, rulesInfo } = toRefs(dataInfo);





function displayEcharts1 () {
  var myChart = echarts.init(content1.value)
  let option = {
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#6a7985'
        }
      }
    },
    grid: {
      top: '20%',
      left: '2%',
      bottom: '1%',
      right: '17%',
      containLabel: true,
    },
    legend: {
      top: '-3%',
      right: '15%',
      icon: 'rect',
      itemWidth: 15, //矩形宽度
      itemHeight: 3, //矩形高度
      textStyle: {
        color: '#DCFFFE',
        fontWeight: 'bold',
        fontFamily: 'Adobe Heiti Std',
      }
    },
    xAxis: {
      splitNumber: 0,
      type: 'category',
      boundaryGap: false,
      data: ['0:00', '4:00', '8:00', '12:00', '16:00', '20:00', '23:00'],
      axisLabel: {
        textStyle: {
          color: '#7ECEF4',
          fontWeight: 400,
          fontFamily: 'MicrosoftYaHei',
        },
      },
      axisLine: {
        lineStyle: {
          type: 'solid',
          color: '#88C4F3',
          width: 1,
        }
      },
      axisTick: {
        show: false
      },
      boundaryGap: false,
    },
    yAxis: {
      max: 20,
      min: 0,
      splitNumber: 4,
      type: 'value',
      splitLine: {
        show: false,
      },
      axisLabel: {
        textStyle: {
          color: '#7ECEF4',
          fontWeight: 400,
          fontFamily: 'MicrosoftYaHei',
        },
      },
      axisLine: {
        show: true,
        lineStyle: {
          type: 'solid',
          color: '#88C4F3',
          width: 1,
        }
      },
    },
    series: [
      //实际率
      {
        name: '实际率',
        data: [13, 8, 10, 12, 11, 9, 11],
        type: 'line',
        areaStyle: {
          opacity: 0.2,
          color: '#91E3F7'
        },
        color: '#00D69C ',
        smooth: true,
        symbol: "none",
        markPoint: {
          symbolSize: 35,
          label: {
            color: '#fff',
          },
          data: [
            { type: 'average', name: 'Average' },
          ]
        },
      },
      // 预测率
      {
        name: '预测率',
        data: [8, 9, 8, 10, 15, 12, 5],
        type: 'line',
        areaStyle: {
          opacity: 0.2,
          color: '#91E3F7'
        },
        color: '#2291E3',
        smooth: true,
        symbol: "none",
        markPoint: {
          symbolSize: 35,
          label: {
            color: '#fff',
          },
          data: [
            { type: 'average', name: 'Average' },
          ]
        },
      }
    ]
  };

  option && myChart.setOption(option);
  window.addEventListener("resize", function () {
    myChart.resize();
  })
}

function displayEcharts2 () {
  var myChart = echarts.init(content2.value)
  let option = {
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#6a7985'
        }
      }
    },
    grid: {
      top: '20%',
      left: '2%',
      bottom: '1%',
      right: '17%',
      containLabel: true,
    },
    legend: {
      itemWidth: 15, //矩形宽度
      itemHeight: 5, //矩形高度
      textStyle: {
        color: '#DCFFFE',
        fontWeight: 'bold',
        fontFamily: 'Adobe Heiti Std',
      }
    },
    xAxis: {
      splitNumber: 0,
      type: 'category',
      boundaryGap: false,
      data: ['2020', '2021', '2022', '2023', '2024', '2025'],
      axisLabel: {
        textStyle: {
          color: '#7ECEF4',
          fontWeight: 400,
          fontFamily: 'MicrosoftYaHei',
        },
      },
      axisLine: {
        lineStyle: {
          type: 'solid',
          color: '#657CA8',
          width: 1,
          opacity: 0.5,
        }
      },
      axisTick: {
        show: false
      },
      boundaryGap: {},
    },
    yAxis: {
      max: 300,
      min: 0,
      splitNumber: 4,
      type: 'value',
      splitLine: {
        show: false,
      },
      axisLabel: {
        textStyle: {
          color: '#7ECEF4',
          fontWeight: 400,
          fontFamily: 'MicrosoftYaHei',
        },
      },
      axisLine: {
        show: true,
        lineStyle: {
          type: 'solid',
          color: '#657CA8',
          width: 1,
          opacity: 0.5,
        }
      },
      splitLine: {
        lineStyle: {
          type: 'solid',
          color: '#fff',
          opacity: 0.1,
        },
      },
    },
    series: [

      // 接入面积
      {
        name: '接入面积',
        data: [140, 165, 190, 215, 240, 265],
        type: 'bar',
        barWidth: '11',
        color: new echarts.graphic.LinearGradient(
          //前四个参数用于配置渐变色的起止位置，这四个参数依次对应 右下左上 四个方位。也就是从右边开始顺时针方向。
          //通过修改前4个参数，可以实现不同的渐变方向
          /*第五个参数则是一个数组，用于配置颜色的渐变过程。
            每项为一个对象，包含offset和color两个参数
          */
          0, 0, 0, 1, [{//代表渐变色从正上方开始
            offset: 0.2, //offset范围是0~1，用于表示位置，0是指0%处的颜色
            color: '#00B7FF'
          }, //柱图渐变色
          {
            offset: 1, //指100%处的颜色
            color: '#2244AC'
          }
        ]),
        itemStyle: {
          borderRadius: 7,//设置圆角
        },
      },
      //开垦面积
      {
        name: '开垦面积',
        data: [75, 110, 100, 120, 90, 170],
        type: 'line',
        color: '#00D69C ',
        smooth: true,
        // symbol: 'roundRect',
        symbolSize: 5,
      },
      // 使用面积
      {
        name: '使用面积',
        data: [45, 80, 60, 100, 75, 150],
        type: 'line',
        color: '#2291E3',
        symbolSize: 5,
        smooth: true,
        // symbol: 'roundRect',
      },

    ]
  };

  option && myChart.setOption(option);
  window.addEventListener("resize", function () {
    myChart.resize();
  })
}


/** 查询农业产业管理列表 */
function getList () {
  loading.value = true;
  listFarming(queryParams.value).then(response => {
    farmingList.value = response.rows;
    total.value = response.total;
    loading.value = false;
    listInfo(queryParamsInfo.value).then(response => {
      infoList.value = response.rows;
      totalInfo.value = response.total;
      loadingInfo.value = false;

      for (let i = 0; i < farmingList.value.length; i++) {
        farmingList.value[i].totalOutput = 0
        for (let j = 0; j < infoList.value.length; j++) {
          if (farmingList.value[i].id == infoList.value[j].farmingId) {
            farmingList.value[i].totalOutput += infoList.value[j].output
          }
        }
      }
      farmingList.value.sort((a, b) => {
        return b.totalOutput - a.totalOutput
      })
      for (let m = 0; m < 3; m++) {
        farmingList.value[m].indexBackImg = new URL("../../../../assets/cockpit/agriculture/右三前三序号背景.png", import.meta.url).href;
      }
      for (let n = 3; n < 5; n++) {
        farmingList.value[n].indexBackImg = new URL("../../../../assets/cockpit/agriculture/右三非前三序号背景.png", import.meta.url).href;
      }
      console.log(farmingList.value[0]);


    });
  });
}
getList();

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
    width: 21vw;
    height: 18.8vh;
    position: relative;
    margin: 2vh 0 3.5vh 2.5vw;
    #content1 {
      width: 100%;
      height: 100%;
    }
  }
  .content-box2 {
    width: 21vw;
    height: 18.8vh;
    position: relative;
    margin: 2vh 0 1vh 2.5vw;
    #content2 {
      width: 100%;
      height: 100%;
    }
  }
  .content-box3 {
    width: 21vw;
    margin-left: 2.5vw;
    .outputValue-box {
      margin-top: 1.2vh;
      width: 17.8vw;
      height: 4vh;
      display: flex;
      align-items: center;
      justify-content: space-around;
      position: relative;
      .img1 {
        position: absolute;
        width: 100%;
        height: 100%;
        margin-top: 1vh;
        z-index: -1;
      }
      .index-box {
        width: 1.3vw;
        height: 2.7vh;
        position: relative;
        margin-left: 1vw;
        .index {
          font-size: 0.8vw;
          color: #ffffff;
          font-weight: bold;
          font-family: Agency FB;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
        }
        img {
          border-radius: 2px;
          width: 100%;
          height: 100%;
        }
      }

      .name {
        color: #ffffff;
        font-family: Microsoft YaHei;
        font-weight: 400;
        font-size: 0.7vw;
        width: 4.8vw;
      }
      .value {
        color: #ffffff;
        font-family: Microsoft YaHei;
        font-weight: 400;
        font-size: 0.7vw;
        width: 3vw;
      }
      .progress {
        opacity: 0.8;
        width: 1.5vw;
      }
      .percentage {
        color: #ffffff;
        font-family: Microsoft YaHei;
        font-weight: 400;
        font-size: 0.7vw;
      }
    }
  }
}
</style>
