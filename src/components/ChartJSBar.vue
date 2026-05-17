<template>
  <div class="chartjs-bar">
    <div class="chart-title">{{ title }}</div>
    <div class="chart-wrap">
      <BarChart :chartData="chartData" :options="chartOptions" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { BarChart } from 'vue-chart-3';
import { Chart, registerables } from 'chart.js';

Chart.register(...registerables);

withDefaults(defineProps<{
  title?: string;
  chartData: any;
}>(), {
  title: 'Reservas por barbero',
});

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: {
      display: false,
    },
    tooltip: {
      backgroundColor: 'rgba(15, 23, 42, 0.9)',
      titleColor: '#c7d0e0',
      bodyColor: '#c7d0e0',
    },
  },
  scales: {
    x: {
      grid: { color: 'rgba(255,255,255,0.05)' },
      ticks: { color: '#8a99b3' },
    },
    y: {
      grid: { color: 'rgba(255,255,255,0.05)' },
      ticks: { color: '#8a99b3' },
      beginAtZero: true,
    },
  },
};
</script>

<style scoped>
.chartjs-bar {
  width: 100%;
  height: 100%;
  padding: 16px;
  display: flex;
  flex-direction: column;
}
.chart-title {
  font-family: 'Rajdhani', sans-serif;
  font-size: 16px;
  font-weight: 700;
  color: #c7d0e0;
  margin-bottom: 12px;
}
.chart-wrap {
  flex: 1;
  position: relative;
  min-height: 0;
}
</style>
