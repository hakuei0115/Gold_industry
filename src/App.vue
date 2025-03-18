<template>
  <div class="app">
    <h1>即時感測數據</h1>
    <div class="grid-container">
      <LiveChart ref="sensor1" sensorKey="sensor1" label="感測器 1" />
      <LiveChart ref="sensor2" sensorKey="sensor2" label="感測器 2" />
      <LiveChart ref="sensor3" sensorKey="sensor3" label="感測器 3" />
      <LiveChart ref="sensor4" sensorKey="sensor4" label="感測器 4" />
      <LiveChart ref="sensor5" sensorKey="sensor5" label="感測器 5" />
      <LiveChart ref="sensor6" sensorKey="sensor6" label="感測器 6" />
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import LiveChart from "../components/LiveChart.vue";

// **正確建立 Vue Ref 來存取 `LiveChart.vue` 組件**
const sensor1 = ref(null);
const sensor2 = ref(null);
const sensor3 = ref(null);
const sensor4 = ref(null);
const sensor5 = ref(null);
const sensor6 = ref(null);

let ws = null;

onMounted(() => {
  ws = new WebSocket("ws://localhost:8000/ws");

  ws.onmessage = (event) => {
    const sensorData = JSON.parse(event.data);
    const timestamp = sensorData.timestamp;

    // **確保每個圖表組件存在並更新數據**
    if (sensor1.value) sensor1.value.updateChart(timestamp, sensorData.sensor1);
    if (sensor2.value) sensor2.value.updateChart(timestamp, sensorData.sensor2);
    if (sensor3.value) sensor3.value.updateChart(timestamp, sensorData.sensor3);
    if (sensor4.value) sensor4.value.updateChart(timestamp, sensorData.sensor4);
    if (sensor5.value) sensor5.value.updateChart(timestamp, sensorData.sensor5);
    if (sensor6.value) sensor6.value.updateChart(timestamp, sensorData.sensor6);
  };

  ws.onerror = (error) => {
    console.error("WebSocket 錯誤:", error);
  };

  ws.onclose = () => {
    console.log("WebSocket 連線已關閉");
  };
});

onUnmounted(() => {
  if (ws) ws.close();
});
</script>

<style scoped>
.app {
  text-align: center;
}

.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  padding: 20px;
}
</style>