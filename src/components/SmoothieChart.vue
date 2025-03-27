<template>
  <div class="chart-container">
    <div class="chart-header">
      <h3 class="chart-title">{{ label }}</h3>
    </div>
    <canvas ref="canvas" :width="width" :height="height"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineExpose } from 'vue'
import { SmoothieChart, TimeSeries } from 'smoothie'

const props = defineProps({
  label: { type: String, default: 'Sensor Chart' },
  width: { type: Number, default: 600 },
  height: { type: Number, default: 200 }
})

const canvas = ref(null)
let chart = null
const flowSeries = new TimeSeries()
const pressureSeries = new TimeSeries()

// 外部透過這個方法傳入數據
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
      fillStyle: '#ffffff',
      lineWidth: 1,
      millisPerLine: 1000,
      verticalSections: 4,
    },
    labels: { fillStyle: '#000' },
    timestampFormatter: date => ''
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

  chart.streamTo(canvas.value, 1000)
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

.chart-header {
  width: 100%;
  max-width: 600px;
  margin-bottom: 6px;
  text-align: left;
}

.chart-title {
  font-weight: bold;
  color: #333;
  font-size: 16px;
}
</style>
