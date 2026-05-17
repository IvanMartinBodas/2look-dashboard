<template>
  <div class="streaming-chart">
    <canvas ref="canvasRef"></canvas>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import { Chart, LineController, LineElement, PointElement, LinearScale, TimeScale, Title, Tooltip, Filler } from 'chart.js';
import streamingPlugin from '@aziham/chartjs-plugin-streaming';
import 'chartjs-adapter-date-fns';

const props = withDefaults(defineProps<{
  chartType?: 'line' | 'area';
  title?: string;
  color?: string;
  min?: number;
  max?: number;
}>(), {
  chartType: 'line',
  title: 'Carga del servidor',
  color: '#10b981',
  min: 0,
  max: 100,
});

const canvasRef = ref<HTMLCanvasElement | null>(null);
let chartInstance: Chart | null = null;

onMounted(() => {
  if (!canvasRef.value) return;
  const isArea = props.chartType === 'area';

  Chart.register(
    LineController, LineElement, PointElement, LinearScale, TimeScale, Title, Tooltip, streamingPlugin
  );
  if (isArea) Chart.register(Filler);

  chartInstance = new Chart(canvasRef.value, {
    type: 'line',
    data: {
      datasets: [{
        label: props.title,
        borderColor: props.color,
        backgroundColor: isArea ? `${props.color}40` : 'transparent',
        borderWidth: 2,
        fill: isArea,
        tension: 0.3,
        data: [],
      }],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      devicePixelRatio: window.devicePixelRatio || 2,
      animation: false,
      scales: {
        x: {
          type: 'realtime',
          realtime: {
            duration: 20000,
            refresh: 1000,
            delay: 1000,
            onRefresh(chart: any) {
              chart.data.datasets[0].data.push({
                x: Date.now(),
                y: Math.floor(Math.random() * (Math.min(props.max, 100) - props.min) + props.min),
              });
            },
          },
          grid: { color: 'rgba(255,255,255,0.05)' },
          ticks: { color: '#8a99b3', maxRotation: 0 },
        },
        y: {
          min: 0,
          max: 100,
          grid: { color: 'rgba(255,255,255,0.05)' },
          ticks: { color: '#8a99b3', stepSize: 20 },
        },
      },
      plugins: {
        legend: { display: false },
        title: {
          display: true,
          text: props.title,
          color: '#c7d0e0',
          align: 'start',
          font: { size: 16, weight: 'bold', family: 'Rajdhani' },
          padding: { top: 4, bottom: 16 },
        },
        tooltip: {
          enabled: true,
          mode: 'nearest',
          intersect: false,
          backgroundColor: 'rgba(15,23,42,0.9)',
          titleColor: '#c7d0e0',
          bodyColor: '#c7d0e0',
          padding: 10,
          displayColors: false,
        },
      },
      elements: {
        point: { radius: 0, hoverRadius: 5, hitRadius: 10, borderWidth: 2 },
        line: { borderWidth: 2, borderJoinStyle: 'round' },
      },
    },
  });
});

onBeforeUnmount(() => {
  if (chartInstance) {
    chartInstance.destroy();
    chartInstance = null;
  }
});
</script>

<style scoped>
.streaming-chart {
  width: 100%;
  height: 100%;
  min-height: 240px;
  padding: 8px 15px;
}
</style>
