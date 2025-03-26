<template>
  <div class="app">
    <h1>SmoothieCharts 模擬數據展示</h1>
    <SmoothieChart ref="sensor1" label="Sensor 1" />
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import SmoothieChart from '../components/SmoothieChart.vue'

const sensor1 = ref(null)
let simulationInterval = null

onMounted(() => {
  // 每 50ms 模擬一筆數據（20Hz）
  simulationInterval = setInterval(() => {
    const fakeFlow = 200 + Math.random() * 50     // 假的 flow
    const fakePressure = 220 + Math.random() * 20 // 假的 pressure
    sensor1.value?.addDataPoint(fakeFlow, fakePressure)
  }, 50)
})

onUnmounted(() => {
  clearInterval(simulationInterval)
})
</script>

<style scoped>
.app {
  padding: 20px;
  text-align: center;
}
</style>
