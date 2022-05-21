<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script setup>
import echarts from 'echarts'
import 'echarts/theme/macarons' // echarts theme

const animationDuration = 3000

defineProps({
  className: {
    type: String,
    default: 'chart'
  },
  width: {
    type: String,
    default: '100%'
  },
  height: {
    type: String,
    default: '300px'
  }
})
const state = reactive({
  chart: null
})

onMounted(() => {
  nextTick(() => {
    initChart()
  })
})

onBeforeUnmount(() => {
  if (!state.chart) {
    return
  }
  state.chart.dispose()
  state.chart = null
})
let { proxy } = getCurrentInstance()
const initChart = () => {
  state.chart = echarts.init(proxy.$el, 'macarons')

  state.chart.setOption({
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        // 坐标轴指示器，坐标轴触发有效
        type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
      }
    },
    radar: {
      radius: '66%',
      center: ['50%', '42%'],
      splitNumber: 8,
      splitArea: {
        areaStyle: {
          color: 'rgba(127,95,132,.3)',
          opacity: 1,
          shadowBlur: 45,
          shadowColor: 'rgba(0,0,0,.5)',
          shadowOffsetX: 0,
          shadowOffsetY: 15
        }
      },
      indicator: [
        { name: '0点', max: 20000 },
        { name: '20点', max: 20000 },
        { name: '16点', max: 20000 },
        { name: '12点', max: 20000 },
        { name: '8点', max: 20000 },
        { name: '4点', max: 20000 }
      ]
    },
    legend: {
      left: 'center',
      bottom: '10',
      data: ['小客车', '公交车', '小货车']
    },
    series: [
      {
        type: 'radar',
        symbolSize: 0,
        areaStyle: {
          normal: {
            shadowBlur: 13,
            shadowColor: 'rgba(0,0,0,.2)',
            shadowOffsetX: 0,
            shadowOffsetY: 10,
            opacity: 1
          }
        },
        data: [
            {value: [5000, 13000, 12000, 13000, 16000, 10000], name: "小客车"},
            {value: [1000, 9000, 12000, 12000, 12000, 11000], name: "公交车"},
            {value: [15500, 12000, 8000, 8000, 9000, 12000], name: "小货车"},
        ],
        animationDuration: animationDuration
      }
    ]
  })
}
</script>

<style scoped lang="scss"></style>
