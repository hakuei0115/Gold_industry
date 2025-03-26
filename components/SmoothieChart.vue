<template>
    <div class="chart-container">
      <h3 class="chart-title">{{ label }}</h3>
      <canvas ref="canvas" width="600" height="200"></canvas>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, onUnmounted, defineExpose } from 'vue'
  import { SmoothieChart, TimeSeries } from 'smoothie'
  
  // Props：外部給圖表名稱
  const props = defineProps({
    label: String,
  })
  
  // 初始化變數
  const canvas = ref(null)
  let chart = null
  const flowSeries = new TimeSeries()
  const pressureSeries = new TimeSeries()
  
  // 提供外部調用的方法
  function addDataPoint(flow, pressure) {
    const now = Date.now()
    flowSeries.append(now, flow)
    pressureSeries.append(now, pressure)
  }
  defineExpose({ addDataPoint })
  
  onMounted(() => {
    chart = new SmoothieChart({
      millisPerPixel: 20, // 每個畫素代表 20 毫秒。值越小，速度越快，資料越「壓縮」。
      grid: {
        strokeStyle: 'rgba(119,119,119,0.2)', // 網格線顏色
        lineWidth: 1, // 網格線寬度
        fillStyle: '#ffffff', // 網格背景顏色
        millisPerLine: 1000, // 每條網格線間隔 1000 毫秒
        verticalSections: 4, // 垂直分段數
      },
      labels: { fillStyle: '#red' }, // 標籤顏色
      timestampFormatter: date => '' // 不顯示時間字串
    })
  
    chart.addTimeSeries(flowSeries, {
      strokeStyle: 'rgba(0, 123, 255, 1)',
      fillStyle: 'rgba(0, 123, 255, 0.1)',
      lineWidth: 2,
    })
  
    chart.addTimeSeries(pressureSeries, {
      strokeStyle: 'rgba(220, 53, 69, 1)',
      fillStyle: 'rgba(220, 53, 69, 0.1)',
      lineWidth: 2,
    })
  
    chart.streamTo(canvas.value, 1000) // 每秒更新畫布
  })
  
  onUnmounted(() => {
    chart.stop()
  })
  </script>
  
  <style scoped>
  .chart-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .chart-title {
    margin-bottom: 8px;
    font-size: 18px;
    font-weight: bold;
    color: #333;
  }
  </style>
  
  