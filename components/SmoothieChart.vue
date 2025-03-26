<template>
    <div class="chart-container">
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
      millisPerPixel: 20,
      grid: {
        strokeStyle: 'rgba(119,119,119,0.2)',
        lineWidth: 1,
        fillStyle: '#ffffff',
        millisPerLine: 1000,
        verticalSections: 4,
      },
      labels: { fillStyle: '#red' },
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
    justify-content: center;
    margin-bottom: 20px;
  }
  </style>
  