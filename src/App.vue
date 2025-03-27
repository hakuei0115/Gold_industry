<template>
  <div class="dashboard">
    <header class="header">
      <h1>即時監控儀表板</h1>
    </header>

    <div class="content">
      <div class="card">
        <!-- 實時數據卡片 -->
        <SensorCard
          label="Sensor 1"
          :flow="sensorData.sensor1.flow"
          :pressure="sensorData.sensor1.pressure"
        />
        <SensorCard
          label="Sensor 2"
          :flow="sensorData.sensor2.flow"
          :pressure="sensorData.sensor2.pressure"
        />
        <SensorCard
          label="Sensor 3"
          :flow="sensorData.sensor3.flow"
          :pressure="sensorData.sensor3.pressure"
        />
        <SensorCard
          label="Sensor 4"
          :flow="sensorData.sensor4.flow"
          :pressure="sensorData.sensor4.pressure"
        />
        <SensorCard
          label="Sensor 5"
          :flow="sensorData.sensor5.flow"
          :pressure="sensorData.sensor5.pressure"
        />
        <SensorCard
          label="Sensor 6"
          :flow="sensorData.sensor6.flow"
          :pressure="sensorData.sensor6.pressure"
        />
      </div>
      <div class="chart">
        <!-- 圖表區 -->
        <SmoothieChart ref="sensorChart1" label="Sensor 1 Chart" />
        <SmoothieChart ref="sensorChart2" label="Sensor 2 Chart" />
        <SmoothieChart ref="sensorChart3" label="Sensor 3 Chart" />
        <SmoothieChart ref="sensorChart4" label="Sensor 4 Chart" />
        <SmoothieChart ref="sensorChart5" label="Sensor 5 Chart" />
        <SmoothieChart ref="sensorChart6" label="Sensor 6 Chart" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import SmoothieChart from './components/SmoothieChart.vue'
import SensorCard from './components/SensorCard.vue'

const sensorChart1 = ref(null)
const sensorChart2 = ref(null)
const sensorChart3 = ref(null)
const sensorChart4 = ref(null)
const sensorChart5 = ref(null)
const sensorChart6 = ref(null)

// 這裡用 sensorData 來保存最新數據，初始為零
const sensorData = ref({
  sensor1: {
    flow: 0,
    pressure: 0
  },
  sensor2: {
    flow: 0,
    pressure: 0
  },
  sensor3: {
    flow: 0,
    pressure: 0
  },
  sensor4: {
    flow: 0,
    pressure: 0
  },
  sensor5: {
    flow: 0,
    pressure: 0
  },
  sensor6: {
    flow: 0,
    pressure: 0
  }
})

// 這裡我們先用假數據模擬，即時更新 sensorData 以及圖表
let simulationInterval = null
onMounted(() => {
  simulationInterval = setInterval(() => {
    const fakeFlow = 200 + Math.random() * 50
    const fakePressure = 220 + Math.random() * 20

    // 更新 SensorCard 的顯示數值
    sensorData.value.sensor1.flow = fakeFlow
    sensorData.value.sensor1.pressure = fakePressure

    sensorData.value.sensor2.flow = fakeFlow
    sensorData.value.sensor2.pressure = fakePressure

    sensorData.value.sensor3.flow = fakeFlow
    sensorData.value.sensor3.pressure = fakePressure

    sensorData.value.sensor4.flow = fakeFlow
    sensorData.value.sensor4.pressure = fakePressure

    sensorData.value.sensor5.flow = fakeFlow
    sensorData.value.sensor5.pressure = fakePressure

    sensorData.value.sensor6.flow = fakeFlow
    sensorData.value.sensor6.pressure = fakePressure

    // 更新圖表：呼叫 SmoothieChart.vue 中暴露的方法
    sensorChart1.value?.addDataPoint(fakeFlow, fakePressure)
    sensorChart2.value?.addDataPoint(fakeFlow, fakePressure)
    sensorChart3.value?.addDataPoint(fakeFlow, fakePressure)
    sensorChart4.value?.addDataPoint(fakeFlow, fakePressure)
    sensorChart5.value?.addDataPoint(fakeFlow, fakePressure)
    sensorChart6.value?.addDataPoint(fakeFlow, fakePressure)
  }, 50) // 模擬 20Hz
})

onUnmounted(() => {
  clearInterval(simulationInterval)
})
</script>

<style scoped>
.dashboard {
  padding: 20px;
  text-align: center;
}

.header {
  margin-bottom: 20px;
}

.card {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}

.chart {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
</style>
