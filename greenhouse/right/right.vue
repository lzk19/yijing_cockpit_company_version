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
        大棚用途分析
      </span>
    </div>
    <!-- 内容盒子1 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->
      <!-- 饼状图 -->
      <div class="div1">
        <div ref="content1"
             id="content1">
        </div>
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
        大棚作物分析
      </span>
    </div>
    <!-- 内容盒子2 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->
      <div class="div2">
        <img src="@/assets/cockpit/greenhouse/右二外框.png"
             alt=""
             class="content2-back">
        <div id="content2"
             ref="content2">
        </div>

      </div>
    </div>

    <!-- 标题盒子3 -->
    <div class="header-box"
         style="margin-top:-1vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="right-content3">

      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        大棚产值排行榜
      </span>
    </div>
    <!-- 内容盒子3 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->

      <div class="ranking"
           v-for="(item,index) in greenhouseList.slice(0,5)"
           :key="index">
        <div class="ranking-index-box">
          <img :src="item.indexBackImg"
               alt=""
               class="ranking-index-img">
          <span class="ranking-index">
            {{index+1}}
          </span>
        </div>
        <div class="ranking-data-box">
          <span class="ranking-name">
            {{item.greenhouseName}}
          </span>
          <span class="ranking-data">
            {{item.cropsValueTotal}}
          </span>
          <img :src="rankBackImg"
               alt=""
               class="ranking-box-img">
        </div>
      </div>
    </div>

  </div>
</template>


<script setup>
import * as echarts from "echarts";
import { ref, onMounted } from "vue";
import { listGreenhouse, getGreenhouse, delGreenhouse, addGreenhouse, updateGreenhouse } from "@/api/basics/greenhouse";
import { listCorpInfo, getCorpInfo, delCorpInfo, addCorpInfo, updateCorpInfo } from "@/api/basics/corpInfo";
import { useRestoreActive } from "element-plus";
const { proxy } = getCurrentInstance();

const greenhouseList = ref([]);
const corpInfoList = ref([]);
const useAreaTotal = ref([{ name: '', value: 0, }, { name: '', value: 0, }, { name: '', value: 0, }]);
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
const rankBackImg = ref(new URL("../../../../assets/cockpit/greenhouse/右三行数据外框.png", import.meta.url).href)
const indexBackImg = ref(new URL("../../../../assets/cockpit/greenhouse/右三橙色序号外框.png", import.meta.url).href,)
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

onMounted(() => {
  init2()
})

function init1 () {
  var myChart1 = echarts.init(content1.value);
  var option1 = {
    title: {
      height: '100',
      text: '总亩数',
      subtext: '9000亩',
      textStyle: {
        borderWidth: 4,
        borderColor: '#fff',
        color: '#88B1D0',
        fontSize: '16',
        fontWeight: 'nomal',
        fontFamily: 'Microsoft YaHei',
      },
      subtextStyle: {
        color: '#ffffff',
        fontSize: '16',
        fontWeight: 'nomal',
        fontFamily: 'DINPro',
      },

      left: 'center',
      top: "38%",
      z: 30
    },
    // tooltip：鼠标移到饼图部分上时显示的对话框
    tooltip: {
      formatter: '{b}：{c}亩',
    },
    series: [

      // 内圆
      {
        type: 'pie',
        radius: '33',
        label: {
          show: false,
        },
        labelLine: {
          show: false
        },
        itemStyle: {

          opacity: 0.8,
          color: {
            type: 'radial',
            x: 0.5,
            y: 0.5,
            r: 0.5,
            colorStops: [{
              offset: 1, color: '#27C9FE' // 0% 处的颜色
            }, {
              offset: 0.35, color: '#0c1330' // 100% 处的颜色
            }],
            global: false // 缺省为 false
          },
          borderColor: '#34ECFF ',
          borderWidth: 3.5,

        },
        data: [
          { name: '总亩数', value: 9000 },
        ],
      },
      // 外环
      {
        hoverAnimation: false,
        type: 'pie',
        radius: ['45', '58'],
        itemStyle: {
          borderRadius: 4,
          borderColor: '#505a7a',
          borderWidth: 3,

        },
        labelLine: {
          length: 10,
          length2: 10,
        },

        label: {

          formatter: '{cValue|{c}亩}\n{bValue|{b}} ',
          borderColor: '#8C8D8E',
          rich: {
            cValue: {
              color: '#FFFFFF',
              fontWeight: 'bold',
              fontFamily: 'DINPro',
              fontSize: 16,
              align: 'center',
            },
            bValue: {
              color: '#88B1D0',
              fontWeight: 'bold',
              fontFamily: 'Microsoft YaHei',
              fontSize: 14,
              align: 'center',
            },
          }
        },
        data: useAreaTotal.value,
      }
    ]


  };


  myChart1.setOption(option1)
  // 默认高亮
  myChart1.dispatchAction({
    type: "highlight",
  });
  window.addEventListener("resize", function () {
    myChart1.resize();
  })
};

function init2 () {
  // 右二折柱混合
  var myChart2 = echarts.init(content2.value);
  var option2 = {
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#6a7985'
        }
      }
    },
    legend: {
      data: ['接入面积', '开通面积', '使用面积'],
      itemWidth: 15, //矩形宽度
      itemHeight: 5, //矩形高度
      // symbol: 'none',
      textStyle: {
        color: '#88B1D0',
        fontWeight: 'bold',
        fontFamily: 'Microsoft YaHei',
      },
    },
    grid: {
      top: '20%',
      left: '3%',
      bottom: '1%',
      right: '7%',
      containLabel: true,
    },
    xAxis: [
      {
        type: 'category',
        data: ['2019', '2020', '2021', '2022', '2023', '2024'],
        axisTick: {
          alignWithLabel: true,
        },
        nameTextStyle: {
          color: '#82b0ec',
        },
        axisLine: {
          show: false,
          lineStyle: {
            color: '#82b0ec',
          },
        },
        axisLabel: {
          textStyle: {
            color: '#BFF7FF',
            fontWeight: 300,
            fontFamily: 'DINPro',
          },
          margin: 10,
        },
      },
    ],
    yAxis: [
      {
        max: 300,
        min: 0,
        splitNumber: 4,
        show: true,
        type: 'value',
        // interval: 2,
        axisLabel: {
          interval: 2,
          opacity: 0.8,
          textStyle: {
            color: '#C3F4FE',
            fontWeight: 300,
            fontFamily: 'DINPro',
          },
        },
        splitLine: {
          interval: 2,
          lineStyle: {
            interval: 2,
            type: 'dotted',
            color: '#fff',
            opacity: 0.1,
          },
        },
      },
    ],
    series: [
      // 上部椭圆
      {
        // name: '无效数据',
        type: 'pictorialBar',
        symbolSize: [15, 7],
        symbolOffset: [0, -5], // 上部椭圆
        symbolPosition: 'end',
        z: 12,
        color: '#21F3FF',
        data: [300, 250, 200, 100, 200, 300],
        itemStyle: {
          opacity: 0.8,
        },
        tooltip: {
          trigger: 'item',
          show: false,
        },
      },
      // 下部椭圆
      {
        // name: '无效数据',
        type: 'pictorialBar',
        symbolSize: [15, 6],
        symbolOffset: [0, 5], // 下部椭圆
        // "barWidth": "20",
        z: 12,
        color: '#1F61EA',
        data: [300, 250, 200, 100, 200, 300],
        itemStyle: {
          opacity: 0.8,
        },
        tooltip: {
          trigger: 'item',
          show: false,
        },
      },
      // 开通面积
      {
        name: '开通面积',
        data: [150, 230, 224, 218, 135, 147],
        type: 'line',
        symbol: 'none',
        lineStyle: {
          color: '#dea908' //改变折线颜色
        }
      },
      // 使用面积
      {
        name: '使用面积',
        data: [250, 130, 124, 118, 235, 247],
        type: 'line',
        symbol: 'none',
        lineStyle: {
          color: '#00BFFF ' //改变折线颜色
        }

      },
      // 接入面积
      {
        name: '接入面积',
        type: 'bar',
        barWidth: '15',
        itemStyle: {
          normal: {
            color: new echarts.graphic.LinearGradient(0, 0, 0, 0.7, [
              {
                offset: 0.4,
                color: '#00B7FF',
              },
              {
                offset: 1,
                color: '#2244AC',
              },
            ]),
            opacity: 0.8,
          },
        },
        data: [300, 250, 200, 100, 200, 300],
      },
    ],
  };
  myChart2.setOption(option2);
  window.addEventListener("resize", function () {
    myChart2.resize();
  })

}

/** 查询智慧大棚管理列表 */
function getList () {
  loading.value = true;
  listGreenhouse(queryParams.value).then(response => {
    greenhouseList.value = response.rows;
    total.value = response.total;
    loading.value = false;

    listCorpInfo(queryParams.value).then(response => {
      corpInfoList.value = response.rows;
      total.value = response.total;
      loading.value = false;
      for (let i = 0; i < greenhouseList.value.length; i++) {
        greenhouseList.value[i].cropsValueTotal = 0
        if (greenhouseList.value[i].greenhouseUse == "1") {

          useAreaTotal.value[0].value += Number(greenhouseList.value[i].greenhouseArea.slice(0, -1))
          useAreaTotal.value[0].name = '水产养殖'
        }
        else if (greenhouseList.value[i].greenhouseUse == "2") {
          useAreaTotal.value[1].value += Number(greenhouseList.value[i].greenhouseArea.slice(0, -1))
          useAreaTotal.value[1].name = "有机蔬菜"
        }
        else if (greenhouseList.value[i].greenhouseUse == "3") {
          useAreaTotal.value[2].value += Number(greenhouseList.value[i].greenhouseArea.slice(0, -1))
          useAreaTotal.value[2].name = "经济作物"
        }
        for (let j = 0; j < corpInfoList.value.length; j++) {
          if (greenhouseList.value[i].id === corpInfoList.value[j].greenhouseId) {
            greenhouseList.value[i].cropsValueTotal += Number(corpInfoList.value[j].corpCapacity)
          }
        }
      }
      greenhouseList.value.sort((a, b) => {
        return b.cropsValueTotal - a.cropsValueTotal
      })
      for (let m = 0; m < 3; m++) {
        greenhouseList.value[m].indexBackImg = new URL("../../../../assets/cockpit/greenhouse/右三橙色序号外框.png", import.meta.url).href;
      }
      for (let n = 3; n < 5; n++) {
        greenhouseList.value[n].indexBackImg = new URL("../../../../assets/cockpit/greenhouse/右三蓝色序号外框.png", import.meta.url).href;
      }
      init1()
    });

  });

}
console.log(useAreaTotal.value);
// console.log(greenhouseList.value);
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
  .content-box {
    width: 21vw;
    position: relative;
    margin: 0 0 2.5vh 2.5vw;
    .div1 {
      height: 18vh;
      width: 21vw;
      #content1 {
        text-align: center;
        margin-left: -1vw;
        width: 100%;
        height: 100%;
      }
    }
    .div2 {
      height: 18vh;
      width: 21vw;
      .content2-back {
        height: 18vh;
        width: 20.5vw;
        margin-left: -1.5vw;
        position: absolute;
        z-index: -1;
      }
      #content2 {
        // text-align: center;
        width: 19vw;
        height: 18vh;
        margin-top: 1.5vh;
      }
    }
    .ranking {
      height: 7vh;
      width: 19vw;
      margin-top: -1vh;
      margin-left: 0.5vw;
      .ranking-index-box {
        width: 2.4vw;
        height: 4.1vh;
        position: relative;
        .ranking-index-img {
          width: 100%;
          height: 100%;
          margin-top: 2vh;
        }
        .ranking-index {
          position: absolute;
          transform: translate(-50%, -20%);
          font-size: 1vw;
          color: #ffffff;
          font-family: Agency FB;
          font-weight: bold;
          top: 80%;
          left: 50%;
        }
      }
      .ranking-data-box {
        display: flex;
        height: 3.6vh;
        width: 19vw;
        margin-top: -0.5vh;
        .ranking-box-img {
          height: 3.6vh;
          width: 19vw;
          margin-left: -16vw;
        }
        .ranking-name {
          color: #c3f4fe;
          font-family: Microsoft YaHei;
          font-weight: 400;
          margin-left: 4vw;
          font-size: 1vw;
          line-height: 3.6vh;
          width: 6vw;
          margin-top: 0.4vh;
        }
        .ranking-data {
          color: #ffffff;
          font-family: DINPro;
          font-weight: 400;
          margin-left: 2.5vw;
          font-size: 1vw;
          line-height: 3.6vh;
          margin-top: 0.4vh;
        }
      }
    }
  }
}
</style>