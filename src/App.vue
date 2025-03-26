<template>
  <div class="app">
    <h1>即時感測器數據</h1>
    <div class="grid-container">
      <LiveChart ref="sensor1" label="Sensor 1" />
      <LiveChart ref="sensor2" label="Sensor 2" />
      <LiveChart ref="sensor3" label="Sensor 3" />
      <LiveChart ref="sensor4" label="Sensor 4" />
      <LiveChart ref="sensor5" label="Sensor 5" />
      <LiveChart ref="sensor6" label="Sensor 6" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import LiveChart from "../components/LiveChart.vue";

// 建立 6 個圖表組件 ref
const sensor1 = ref(null);
const sensor2 = ref(null);
const sensor3 = ref(null);
const sensor4 = ref(null);
const sensor5 = ref(null);
const sensor6 = ref(null);

let ws = null;
let latestSensorData = null; // 緩衝區，只保留最新資料

onMounted(() => {
  // 建立 WebSocket 連線
  ws = new WebSocket("ws://localhost:8000/ws");

  ws.onmessage = (event) => {
    const data = JSON.parse(event.data);
    latestSensorData = data; // 更新緩衝區
  };

  ws.onerror = (error) => {
    console.error("WebSocket 錯誤:", error);
  };

  ws.onclose = () => {
    console.log("WebSocket 連線已關閉");
  };

  // 每 100ms 更新一次圖表（前端更新頻率 = 10Hz）
  const chartUpdateInterval = setInterval(() => {
    if (!latestSensorData) return;

    const { timestamp, ...sensors } = latestSensorData;

    sensor1.value?.updateChart(timestamp, sensors.sensor1);
    sensor2.value?.updateChart(timestamp, sensors.sensor2);
    sensor3.value?.updateChart(timestamp, sensors.sensor3);
    sensor4.value?.updateChart(timestamp, sensors.sensor4);
    sensor5.value?.updateChart(timestamp, sensors.sensor5);
    sensor6.value?.updateChart(timestamp, sensors.sensor6);
  }, 100); // 每 100ms 更新一次

  // 清理
  onUnmounted(() => {
    if (ws) ws.close();
    clearInterval(chartUpdateInterval);
  });
});
</script>

<style scoped>
.app {
  text-align: center;
  padding: 20px;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  padding: 20px;
}
</style>
