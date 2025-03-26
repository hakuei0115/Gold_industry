<template>
  <div class="chart-container">
    <canvas ref="chartCanvas"></canvas>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, defineExpose } from "vue";
import { Chart, registerables } from "chart.js";

Chart.register(...registerables);

// 接收父層傳入的標題用於圖表 label
const props = defineProps({
  label: String
});

const chartCanvas = ref(null);
let chartInstance = null;

// 初始化圖表
onMounted(() => {
  if (!chartCanvas.value) return;

  chartInstance = new Chart(chartCanvas.value, {
    type: "line",
    data: {
      labels: [], // 時間
      datasets: [
        {
          label: `${props.label} - Flow`,
          data: [],
          borderColor: "rgba(54, 162, 235, 1)", // 藍色
          backgroundColor: "rgba(54, 162, 235, 0.2)",
          borderWidth: 2,
          tension: 0.3,
        },
        {
          label: `${props.label} - Pressure`,
          data: [],
          borderColor: "rgba(255, 99, 132, 1)", // 紅色
          backgroundColor: "rgba(255, 99, 132, 0.2)",
          borderWidth: 2,
          tension: 0.3,
        },
      ],
    },
    options: {
      responsive: true,
      animation: false,
      plugins: {
        legend: {
          display: true,
        },
      },
      scales: {
        x: {
          title: { display: true, text: "時間" },
          ticks: { display: false }
        },
        y: {
          title: { display: true, text: "數值" },
          beginAtZero: true
        }
      }
    }
  });
});

// 提供給父層更新圖表的方法
function updateChart(timestamp, { flow, pressure }) {
  const labels = chartInstance.data.labels;
  const flowData = chartInstance.data.datasets[0].data;
  const pressureData = chartInstance.data.datasets[1].data;

  const time = new Date(timestamp * 1000).toLocaleTimeString();

  if (labels.length > 50) {
    labels.shift();
    flowData.shift();
    pressureData.shift();
  }

  labels.push(time);
  flowData.push(flow);
  pressureData.push(pressure);

  chartInstance.update();
}

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
