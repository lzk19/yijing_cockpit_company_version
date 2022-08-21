<template>

  <div class="box1">
    <img src="@/assets/cockpit/public/左侧导航背景.png"
         alt=""
         class="left">
    <div class="title-box">
      <span>
        <!-- 大标题填写处 -->
        农业产业
      </span>
      <img src="src\assets\cockpit\public\椭圆 1936.png"
           alt=""
           class="left-ellipse">
    </div>

    <!-- 标题盒子1 -->
    <div class="header-box"
         style="margin-top:-1vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content1">
      <!-- 圆点 -->
      <div class="status-point" />
      <!-- 标题 -->
      <span class="span1">
        <!-- 内容标题填写处 -->
        农业产量汇总
      </span>
    </div>
    <!-- 内容盒子1 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->
      <div id="main1"
           ref="main1"></div>

    </div>

    <!-- 标题盒子2 -->
    <div class="header-box"
         style="margin-top:-5vh">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content2">
      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        农业效能分析
      </span>
    </div>
    <!-- 内容盒子2 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->
      <div id="main2"
           ref="main2"></div>
    </div>
    <!-- 标题盒子3 -->
    <div class="header-box">
      <img src="@/assets/cockpit/public/具体内容标题背景.png"
           alt=""
           class="left-content3">

      <div class="status-point" />
      <span>
        <!-- 内容标题填写处 -->
        销售列表
      </span>
    </div>
    <!-- 内容盒子3 -->
    <div class="content-box">
      <!-- 具体内容填写处 -->

      <div class="list-box"
           v-for="(item,index) in infoList.slice(0,8)"
           :key="index">
        <div class="restore">
          <div class="list-index-box">
            <img :src="item.indexBackImg"
                 alt="">
            <span class="list-index">
              NO.{{index+1}}
            </span>
          </div>
          <span class="list-name">
            {{item.productName}}
          </span>
          <span class="list-data">
            {{item.salesVolume}}
          </span>
        </div>
      </div>

    </div>

  </div>
</template>

<script setup>
import * as echarts from 'echarts'
import 'echarts-gl'
import { listFarming, getFarming, delFarming, addFarming, updateFarming } from "@/api/basics/farming";
import { listInfo, getInfo, delInfo, addInfo, updateInfo } from "@/api/basics/info";
const { proxy } = getCurrentInstance();

// const indexBackImg = ref(new URL("../../../../assets/cockpit/agriculture/左三NO.1.png", import.meta.url).href,)
const main1 = ref()
const main2 = ref()
const farmingList = ref([]);
const open = ref(false);
const loading = ref(true);
const showSearch = ref(true);
const ids = ref([]);
const multiple = ref(true);
const total = ref(0);
const title = ref("");
const agricultureOutputSum = ref([{ name: '', value: 0, }, { name: '', value: 0, }])


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



onMounted(() => {

  displayEcharts2();
})

function displayEcharts1 () {
  var myChart = echarts.init(main1.value);
  function getParametricEquation (startRatio, endRatio, isSelected, isHovered, k, h) {

    // 计算
    let midRatio = (startRatio + endRatio) / 2;

    let startRadian = startRatio * Math.PI * 2;
    let endRadian = endRatio * Math.PI * 2;
    let midRadian = midRatio * Math.PI * 2;

    // 如果只有一个扇形，则不实现选中效果。
    if (startRatio === 0 && endRatio === 1) {
      isSelected = false;
    }

    // 通过扇形内径/外径的值，换算出辅助参数 k（默认值 1/3）
    k = typeof k !== 'undefined' ? k : 1 / 3;

    // 计算选中效果分别在 x 轴、y 轴方向上的位移（未选中，则位移均为 0）
    let offsetX = isSelected ? Math.cos(midRadian) * 0.1 : 0;
    let offsetY = isSelected ? Math.sin(midRadian) * 0.1 : 0;

    // 计算高亮效果的放大比例（未高亮，则比例为 1）
    let hoverRate = isHovered ? 1.05 : 1;

    // 返回曲面参数方程
    return {

      u: {
        min: -Math.PI,
        max: Math.PI * 3,
        step: Math.PI / 32
      },

      v: {
        min: 0,
        max: Math.PI * 2,
        step: Math.PI / 20
      },

      x: function (u, v) {
        if (u < startRadian) {
          return offsetX + Math.cos(startRadian) * (1 + Math.cos(v) * k) * hoverRate;
        }
        if (u > endRadian) {
          return offsetX + Math.cos(endRadian) * (1 + Math.cos(v) * k) * hoverRate;
        }
        return offsetX + Math.cos(u) * (1 + Math.cos(v) * k) * hoverRate;
      },

      y: function (u, v) {
        if (u < startRadian) {
          return offsetY + Math.sin(startRadian) * (1 + Math.cos(v) * k) * hoverRate;
        }
        if (u > endRadian) {
          return offsetY + Math.sin(endRadian) * (1 + Math.cos(v) * k) * hoverRate;
        }
        return offsetY + Math.sin(u) * (1 + Math.cos(v) * k) * hoverRate;
      },

      z: function (u, v) {
        if (u < -Math.PI * 0.5) {
          return Math.sin(u);
        }
        if (u > Math.PI * 2.5) {
          return Math.sin(u) * h * .1;
        }
        return Math.sin(v) > 0 ? 1 * h * .1 : -1;
      }
    };
  }

  // 生成模拟 3D 饼图的配置项
  function getPie3D (pieData, internalDiameterRatio) {

    let series = [];
    let sumValue = 0;
    let startValue = 0;
    let endValue = 0;
    let legendData = [];
    let k = typeof internalDiameterRatio !== 'undefined' ? (1 - internalDiameterRatio) / (1 + internalDiameterRatio) : 1 / 3;

    // 为每一个饼图数据，生成一个 series-surface 配置
    for (let i = 0; i < pieData.length; i++) {

      sumValue += pieData[i].value;

      let seriesItem = {
        name: typeof pieData[i].name === 'undefined' ? `series${i}` : pieData[i].name,
        type: 'surface',
        parametric: true,
        wireframe: {
          show: false
        },
        pieData: pieData[i],
        pieStatus: {
          selected: false,
          hovered: false,
          k: k
        }
      };

      if (typeof pieData[i].itemStyle != 'undefined') {

        let itemStyle = {};

        typeof pieData[i].itemStyle.color != 'undefined' ? itemStyle.color = pieData[i].itemStyle.color : null;
        typeof pieData[i].itemStyle.opacity != 'undefined' ? itemStyle.opacity = pieData[i].itemStyle.opacity : null;

        seriesItem.itemStyle = itemStyle;
      }
      series.push(seriesItem);
    }

    // 使用上一次遍历时，计算出的数据和 sumValue，调用 getParametricEquation 函数，
    // 向每个 series-surface 传入不同的参数方程 series-surface.parametricEquation，也就是实现每一个扇形。
    let linesSeries = [];
    for (let i = 0; i < series.length; i++) {
      endValue = startValue + series[i].pieData.value;

      series[i].pieData.startRatio = startValue / sumValue;
      series[i].pieData.endRatio = endValue / sumValue;
      series[i].parametricEquation = getParametricEquation(series[i].pieData.startRatio, series[i].pieData.endRatio, false, false, k, series[i].pieData.value);

      startValue = endValue;
      let midRadian = (series[i].pieData.endRatio + series[i].pieData.startRatio) * Math.PI;
      let posX = Math.cos(midRadian) * (1 + Math.cos(Math.PI / 2));
      let posY = Math.sin(midRadian) * (1 + Math.cos(Math.PI / 2));
      let posZ = Math.log(Math.abs(series[i].pieData.value + 1));
      let flag = ((midRadian >= 0 && midRadian <= Math.PI / 2) || (midRadian >= 3 * Math.PI / 2 && midRadian <= Math.PI * 2)) ? 1 : -1;
      let color = pieData[i].itemStyle.color;
      let endPosArr = [posX * (1.8) + (i * 0.1 * flag) + (flag < 0 ? -1 : 0), posY * (2.5) + (i * 0.1 * flag) + (flag < 0 ? -1 : 0), posZ * (10)]
      linesSeries.push({
        type: 'line3D',
        lineStyle: {
          color: color,
        },
        data: [[posX, posY, posZ], endPosArr]
      }, {
        type: 'scatter3D',
        label: {
          show: true,
          textStyle: {
            color: 'white',

            // backgroundColor: 'transparent'
          },
          formatter: '{b}'
        },
        symbolSize: 0,
        data: [{ name: series[i].name + '/n' + series[i].pieData.value, value: endPosArr }]
      });
      legendData.push(series[i].name);
    }
    // series = series.concat(linesSeries)

    // 补充一个透明的圆环，用于支撑高亮功能的近似实现。
    series.push({

      name: 'mouseoutSeries',
      type: 'surface',
      parametric: true,
      wireframe: {
        show: false
      },
      itemStyle: {
        opacity: 0,
      },
      parametricEquation: {
        u: {
          min: 0,
          max: Math.PI * 2,
          step: Math.PI / 20
        },
        v: {
          min: 0,
          max: Math.PI,
          step: Math.PI / 20
        },
        x: function (u, v) {
          return Math.sin(v) * Math.sin(u) + Math.sin(u);
        },
        y: function (u, v) {
          return Math.sin(v) * Math.cos(u) + Math.cos(u);
        },
        z: function (u, v) {
          return Math.cos(v) > 0 ? 0.1 : -0.1;
        }
      }
    });

    // 准备待返回的配置项，把准备好的 legendData、series 传入。
    let option = {
      legend: {
        data: legendData,
        top: -5,
        textStyle: {
          color: '#DCFFFE',
          fontFamily: 'Adobe Heiti Std',
          fontSize: 12,
          fontWeight: 'bold',
        }
      },
      tooltip: {
        formatter: params => {
          if (params.seriesName !== 'mouseoutSeries') {
            return `${params.seriesName}<br/><span style="display:inline-block;margin-right:5px;border-radius:10px;width:10px;height:10px;background-color:${params.color};"></span>${option.series[params.seriesIndex].pieData.value}kg`;
          }
        }
      },
      xAxis3D: {
        min: -1,
        max: 1
      },
      yAxis3D: {
        min: -1,
        max: 1
      },
      zAxis3D: {
        min: -1,
        max: 1
      },
      grid3D: {
        show: false,
        boxHeight: 0.005,
        bottom: '50%',
        viewControl: {//3d效果可以放大、旋转等，请自己去查看官方配置
          alpha: 35,
          beta: 110,
          distance: 150,
          rotateSensitivity: 0,
          zoomSensitivity: 0,
          panSensitivity: 0,
          autoRotate: false
        },
        //后处理特效可以为画面添加高光、景深、环境光遮蔽（SSAO）、调色等效果。可以让整个画面更富有质感。
        postEffect: {//配置这项会出现锯齿，请自己去查看官方配置有办法解决
          enable: true,
          bloom: {
            enable: true,
            bloomIntensity: 0.1
          },
          SSAO: {
            enable: true,
            quality: 'medium',
            radius: 2
          }
        }
      },
      series: series
    };
    return option;
  }

  // 传入数据生成 option
  // 数据显示顺序是从右下逆时针到左
  let option = getPie3D([{
    name: agricultureOutputSum.value[0].name,
    value: agricultureOutputSum.value[0].value,
    itemStyle: {
      opacity: 0.45,
    }
  }, {
    name: agricultureOutputSum.value[1].name,
    value: agricultureOutputSum.value[1].value,
    itemStyle: {
      opacity: 0.45,
    }
  },], 1.618);


  // 监听鼠标事件，实现饼图选中效果（单选），近似实现高亮（放大）效果。
  let selectedIndex = '';
  let hoveredIndex = '';

  // 监听点击事件，实现选中效果（单选）
  myChart.on('click', function (params) {
    // 从 option.series 中读取重新渲染扇形所需的参数，将是否选中取反。
    let isSelected = !option.series[params.seriesIndex].pieStatus.selected;
    let isHovered = option.series[params.seriesIndex].pieStatus.hovered;
    let k = option.series[params.seriesIndex].pieStatus.k;
    let startRatio = option.series[params.seriesIndex].pieData.startRatio;
    let endRatio = option.series[params.seriesIndex].pieData.endRatio;


    // 如果之前选中过其他扇形，将其取消选中（对 option 更新）
    if (selectedIndex !== '' && selectedIndex !== params.seriesIndex) {
      option.series[selectedIndex].parametricEquation = getParametricEquation(option.series[selectedIndex].pieData.startRatio, option.series[selectedIndex].pieData.endRatio, false, false, k, option.series[selectedIndex].pieData.value);
      option.series[selectedIndex].pieStatus.selected = false;
    }

    // 对当前点击的扇形，执行选中/取消选中操作（对 option 更新）
    option.series[params.seriesIndex].parametricEquation = getParametricEquation(startRatio, endRatio, isSelected, isHovered, k, option.series[selectedIndex].pieData.value);
    option.series[params.seriesIndex].pieStatus.selected = isSelected;

    // 如果本次是选中操作，记录上次选中的扇形对应的系列号 seriesIndex
    isSelected ? selectedIndex = params.seriesIndex : null;

    // 使用更新后的 option，渲染图表
    myChart.setOption(option);
  });

  // 监听 mouseover，近似实现高亮（放大）效果
  myChart.on('mouseover', function (params) {

    // 准备重新渲染扇形所需的参数
    let isSelected;
    let isHovered;
    let startRatio;
    let endRatio;
    let k;

    // 如果触发 mouseover 的扇形当前已高亮，则不做操作
    if (hoveredIndex === params.seriesIndex) {
      return;

      // 否则进行高亮及必要的取消高亮操作
    } else {

      // 如果当前有高亮的扇形，取消其高亮状态（对 option 更新）
      if (hoveredIndex !== '') {
        // 从 option.series 中读取重新渲染扇形所需的参数，将是否高亮设置为 false。
        isSelected = option.series[hoveredIndex].pieStatus.selected;
        isHovered = false;
        startRatio = option.series[hoveredIndex].pieData.startRatio;
        endRatio = option.series[hoveredIndex].pieData.endRatio;
        k = option.series[hoveredIndex].pieStatus.k;

        // 对当前点击的扇形，执行取消高亮操作（对 option 更新）
        option.series[hoveredIndex].parametricEquation = getParametricEquation(startRatio, endRatio, isSelected, isHovered, k, option.series[hoveredIndex].pieData.value);
        option.series[hoveredIndex].pieStatus.hovered = isHovered;

        // 将此前记录的上次选中的扇形对应的系列号 seriesIndex 清空
        hoveredIndex = '';
      }

      // 如果触发 mouseover 的扇形不是透明圆环，将其高亮（对 option 更新）
      if (params.seriesName !== 'mouseoutSeries') {
        // 从 option.series 中读取重新渲染扇形所需的参数，将是否高亮设置为 true。
        isSelected = option.series[params.seriesIndex].pieStatus.selected;
        isHovered = true;
        startRatio = option.series[params.seriesIndex].pieData.startRatio;
        endRatio = option.series[params.seriesIndex].pieData.endRatio;
        k = option.series[params.seriesIndex].pieStatus.k;

        // 对当前点击的扇形，执行高亮操作（对 option 更新）
        option.series[params.seriesIndex].parametricEquation = getParametricEquation(startRatio, endRatio, isSelected, isHovered, k, option.series[params.seriesIndex].pieData.value + 5);
        option.series[params.seriesIndex].pieStatus.hovered = isHovered;

        // 记录上次高亮的扇形对应的系列号 seriesIndex
        hoveredIndex = params.seriesIndex;
      }

      // 使用更新后的 option，渲染图表
      myChart.setOption(option);
    }
  });

  // 修正取消高亮失败的 bug
  myChart.on('globalout', function () {


    if (hoveredIndex !== '') {
      // 从 option.series 中读取重新渲染扇形所需的参数，将是否高亮设置为 true。
      isSelected = option.series[hoveredIndex].pieStatus.selected;
      isHovered = false;
      k = option.series[hoveredIndex].pieStatus.k;
      startRatio = option.series[hoveredIndex].pieData.startRatio;
      endRatio = option.series[hoveredIndex].pieData.endRatio;

      // 对当前点击的扇形，执行取消高亮操作（对 option 更新）
      option.series[hoveredIndex].parametricEquation = getParametricEquation(startRatio, endRatio, isSelected, isHovered, k, option.series[hoveredIndex].pieData.value);
      option.series[hoveredIndex].pieStatus.hovered = isHovered;

      // 将此前记录的上次选中的扇形对应的系列号 seriesIndex 清空
      hoveredIndex = '';
    }

    // 使用更新后的 option，渲染图表
    myChart.setOption(option);
  });

  option && myChart.setOption(option);
  window.addEventListener("resize", function () {
    myChart.resize();
  })

};

function displayEcharts2 () {
  var myChart = echarts.init(main2.value)
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
      right: '10%',
      containLabel: true,
    },
    legend: {
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
      data: ['05/10', '05/11', '05/12', '05/13', '05/14', '05/15', '05/16'],
      axisLabel: {
        textStyle: {
          color: '#88C4F3',
          fontWeight: 'bold',
          fontFamily: 'Adobe Heiti Std',
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
      boundaryGap: ['20%', '20%']
    },
    yAxis: {
      name: 'kWh/m²',
      nameLocation: 'end',
      // nameGap: -25,
      nameTextStyle: {
        color: '#DCFFFE ',
        fontWeight: 'bold',
        fontFamily: 'Adobe Heiti Std',
        fontSize: 10,
      },
      type: 'value',
      splitLine: {
        show: false,
      },
      axisLabel: {
        textStyle: {
          color: '#88C4F3',
          fontWeight: 'bold',
          fontFamily: 'SourceHanSansCN',
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
      //实际耗能折线
      {
        name: '实际耗能',
        data: [0.13, 0.23, 0.33, 0.37, 0.13, 0.33, 0.23],
        type: 'line',
        areaStyle: {
          opacity: 0.1,
          color: '#03DAFD'
        },
        color: '#23acfa',
        symbol: "image://src/assets/cockpit/agriculture/左二折线图拐点.png",
        symbolSize: 12,//拐点大小
        showAllSymbol: true,
        smooth: 1,
        // itemStyle: { normal: { label: { show: true } } }
      },
      // 预测耗能折线
      {
        name: '预测耗能',
        data: [0.27, 0.37, 0.30, 0.17, 0.27, 0.20, 0.35],
        type: 'line',
        areaStyle: {
          opacity: 0.1,
          color: '#03DAFD'
        },
        color: '#07cff0',
        symbol: "image://src/assets/cockpit/agriculture/左二折线图拐点.png",
        symbolSize: 12,//拐点大小
        showAllSymbol: true,
        smooth: 1,
      }
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

      infoList.value.sort((a, b) => {
        return b.salesVolume - a.salesVolume
      })
      infoList.value[0].indexBackImg = new URL("../../../../assets/cockpit/agriculture/左三NO.1.png", import.meta.url).href;
      infoList.value[1].indexBackImg = new URL("../../../../assets/cockpit/agriculture/左三NO.2.png", import.meta.url).href;
      infoList.value[2].indexBackImg = new URL("../../../../assets/cockpit/agriculture/左三NO.3.png", import.meta.url).href;
      for (let i = 3; i < 8; i++) {
        infoList.value[i].indexBackImg = new URL("../../../../assets/cockpit/agriculture/左三NO.4.png", import.meta.url).href;
      }

      for (let m = 0; m < farmingList.value.length; m++) {
        farmingList.value[m].totalOutput = 0
        for (let n = 0; n < infoList.value.length; n++) {
          if (farmingList.value[m].id == infoList.value[n].farmingId) {
            farmingList.value[m].totalOutput += infoList.value[n].output
          }
        }
      }
      // console.log(agricultureOutputSum.value);
      for (let z = 0; z < farmingList.value.length; z++) {
        if (farmingList.value[z].farmingType == '1') {
          agricultureOutputSum.value[0].name = '蔬菜产业'
          agricultureOutputSum.value[0].value += Number(farmingList.value[z].totalOutput)
        }
        else if (farmingList.value[z].farmingType == '2') {
          agricultureOutputSum.value[1].name = '水果产业'
          agricultureOutputSum.value[1].value += Number(farmingList.value[z].totalOutput)
        }
      }
      agricultureOutputSum.value.sort((a, b) => {
        return a.value - b.value
      })
      console.log(agricultureOutputSum.value);
      // console.log(infoList.value[0]);
      // 1是蔬菜 2是水果
      // for(let z=0;z<infoList.value.length;z++){
      //   if(infoList.value[z].farmingId)
      // }
      // console.log(farmingList.value[infoList.value[0].farmingId].userId);
      console.log(farmingList.value);
      displayEcharts1();
      // console.log(infoList.value);
    });
    // console.log(infoList.value[0]);
    // console.log(infoList.value);
  });
}
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
    margin-left: 0.5vw;
    display: flex;
    height: 4vh;
    align-items: center; //垂直
    width: 21.9vw;
    img {
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
      margin-left: 0.75vw;
    }
    span {
      line-height: 4vh;
      margin-left: 1.5vw;
      height: 4vh;
      width: 10vw;
      font-size: 0.83vw;
      font-family: Alibaba PuHuiTi;
      font-weight: bold;
      color: #ffffff;
    }
  }
  .content-box {
    width: 21vw;
    height: 18.8vh;
    position: relative;
    margin-top: 1vh;
    margin-bottom: 1.5vh;
    #main1 {
      width: 100%;
      height: 100%;
    }
    #main2 {
      // text-align: center;
      width: 21vw;
      height: 20vh;
    }
    .list-box {
      margin-left: 1.5vw;
      margin-top: 1vh;
      width: 16.25vw;
      height: 2.7vh;
      // opacity: 0.8;
      background: linear-gradient(
        90deg,
        rgba(56, 101, 157, 0.5),
        rgba(43, 95, 143, 0)
      );
      transform: skewX(-20deg);
      .restore {
        display: flex;
        width: 16.25vw;
        height: 2.7vh;
        transform: skewX(20deg);
        .list-index-box {
          width: 3.2vw;
          height: 2.7vh;
          display: flex;
          margin-left: -0.1vw;
          justify-content: center;
          align-items: center;
          .list-index {
            font-size: 0.8vw;
            font-family: BebasNeueCyrillic;
            font-weight: bold;
            font-style: italic;
            color: #ffffff;
            opacity: 0.7;
            margin-left: -2.7vw;
          }
          img {
            width: 3.2vw;
            height: 2.7vh;
          }
        }
        .list-name {
          color: #bcf0fe;
          font-size: 0.9vw;
          font-family: MicrosoftYaHei;
          margin-left: 3.5vw;
          width: 3vw;
        }
        .list-data {
          color: #1ac9ff;
          font-size: 0.9vw;
          font-family: MicrosoftYaHei-Bold;
          font-weight: bold;
          width: 3vw;
          margin-left: 3vw;
        }
      }
    }
  }
}
</style>
