<template>
  <div class="box-mixedChart">
    <VueApexCharts type="line" height="100%" :options="options" :series="series" />
  </div>
</template>

<script setup lang="ts">
import VueApexCharts from 'vue3-apexcharts';
import type { ApexOptions } from 'apexcharts';

const props = withDefaults(defineProps<{
  series: any;
  title?: string;
  labels?: string[];
}>(), {
  title: 'Ingresos y reservas',
  labels: () => ['S1', 'S2', 'S3', 'S4'],
});

const options: ApexOptions = {
  chart: {
    type: 'line',
    stacked: false,
    background: 'transparent',
    toolbar: { show: false },
    foreColor: '#c7d0e0',
  },
  theme: { mode: 'dark' },
  title: {
    text: props.title,
    align: 'left',
    style: {
      fontSize: '16px',
      fontWeight: '700',
      color: '#c7d0e0',
      fontFamily: 'Rajdhani, sans-serif',
    },
  },
  colors: ['#4d8cff', '#10b981', '#f59e0b'],
  stroke: {
    width: [0, 2, 4],
    curve: 'smooth',
  },
  plotOptions: {
    bar: { columnWidth: '40%', borderRadius: 4 },
  },
  fill: {
    opacity: [0.9, 0.25, 1],
    gradient: {
      inverseColors: false,
      shade: 'dark',
      type: 'vertical',
      opacityFrom: 0.7,
      opacityTo: 0.2,
      stops: [0, 100, 100, 100],
    },
  },
  labels: props.labels,
  markers: { size: 4, strokeWidth: 0 },
  xaxis: {
    labels: { style: { colors: '#8a99b3' } },
    axisBorder: { color: '#1f2a44' },
    axisTicks: { color: '#1f2a44' },
  },
  yaxis: {
    labels: { style: { colors: '#8a99b3' } },
  },
  grid: {
    show: true,
    borderColor: '#1f2a44',
    strokeDashArray: 4,
  },
  legend: {
    position: 'top',
    horizontalAlign: 'right',
    labels: { colors: '#c7d0e0' },
  },
  tooltip: {
    shared: true,
    theme: 'dark',
    intersect: false,
  },
};
</script>

<style scoped>
.box-mixedChart {
  display: flex;
  flex-direction: column;
  height: 100%;
  min-height: 280px;
  width: 100%;
  padding: 16px;
}
</style>
