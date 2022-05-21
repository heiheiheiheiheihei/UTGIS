<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script setup>
import { onMounted, getCurrentInstance, reactive, onBeforeUnmount } from 'vue'
import echarts from 'echarts'
//获取store和router
// import {useRouter} from 'vue-router'
// import {useStore} from 'vuex'
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

//const routes = computed(() => {
//  });
// watch(() => props.name, (oldValue,newValue) => {
//
//   },
//   { immediate: true }
// );

// const store = useStore()
// const router = useRouter()
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

const { proxy } = getCurrentInstance()
const initChart = () => {
  state.chart = echarts.init(proxy.$el, 'macarons')
  state.chart.setOption({
    tooltip: {
      trigger: 'item',
      formatter: '{a} <br/>{b} : {c} ({d}%)'
    },
    legend: {
      left: 'center',
      bottom: '10',
      data: ['小客车', '公交车', '小货车', '大货车', '特种车辆']
    },
    series: [
      {
        name: '车流量构成',
        type: 'pie',
        roseType: 'radius',
        radius: [15, 95],
        center: ['50%', '38%'],
        data: [
          { value: 320, name: '小客车' },
          { value: 240, name: '公交车' },
          { value: 149, name: '小货车' },
          { value: 100, name: '大货车' },
          { value: 59, name: '特种车辆' }
        ],
        animationEasing: 'cubicInOut',
        animationDuration: 2600
      }
    ]
  })
}
</script>

<style scoped lang="scss"></style>
