<template>
  <div class="chart-container">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineExpose } from "vue";
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);

const props = defineProps({
  sensorKey: String,  // 傳入的感測器名稱
  label: String       // 圖表標題
});

const chartCanvas = ref(null);
let chartInstance = null;

onMounted(() => {
  if (!chartCanvas.value) return;

  // 創建 Chart.js 折線圖
  chartInstance = new Chart(chartCanvas.value, {
    type: "line",
    data: {
      labels: [], // X 軸時間
      datasets: [
        {
          label: props.label,
          data: [], // Y 軸數值
          borderColor: "rgba(75, 192, 192, 1)",
          backgroundColor: "rgba(75, 192, 192, 0.2)",
          borderWidth: 2,
          tension: 0.2, // 平滑折線
        },
      ],
    },
    options: {
      responsive: true,
      animation: false, // 禁用動畫提高效能
      scales: {
        x: {
          title: { display: true, text: "時間" },
          ticks: { display: false },
        },
        y: {
          title: { display: true, text: "數值" },
          beginAtZero: true,
        },
      },
    },
  });
});

// 更新折線圖
function updateChart(timestamp, value) {
  if (!chartInstance) return;

  const chartData = chartInstance.data;
  if (chartData.labels.length > 50) {
    chartData.labels.shift();
    chartData.datasets[0].data.shift();
  }

  chartData.labels.push(new Date(timestamp * 1000).toLocaleTimeString());
  chartData.datasets[0].data.push(value);

  chartInstance.update();
}

// **確保 `updateChart` 方法可被 `App.vue` 調用**
defineExpose({ updateChart });

onUnmounted(() => {
  if (chartInstance) chartInstance.destroy();
});
</script>

<style scoped>
.chart-container {
  width: 100%;
  height: 250px;
}
</style>
