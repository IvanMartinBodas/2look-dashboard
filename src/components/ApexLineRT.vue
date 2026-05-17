<template>
  <div class="box-realTimeChart">
    <VueApexCharts height="100%" :options="chartOptions" :series="series" />
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, watch, nextTick } from 'vue';
import type { ApexOptions } from 'apexcharts';
import VueApexCharts from 'vue3-apexcharts';

const props = defineProps<{
  series: { name: string; data: { x: number; y: number }[] }[];
  title?: string;
  kpiTarget?: number;
  color?: string;
}>();

const title = computed(() => props.title ?? 'Usuarios online');
const chartColor = computed(() => props.color ?? '#4d8cff');
const kpiTarget = computed(() => props.kpiTarget ?? 70);

const forceResize = () => {
  nextTick(() => {
    setTimeout(() => window.dispatchEvent(new Event('resize')), 300);
  });
};

onMounted(() => { forceResize(); });

let hasResizedOnData = false;
watch(
  () => props.series?.[0]?.data?.length,
  (len) => {
    if (len && len >= 1 && !hasResizedOnData) {
      hasResizedOnData = true;
      forceResize();
    }
  }
);

const chartOptions = computed<ApexOptions>(() => {
  const data = props.series?.[0]?.data ?? [];
  const lastX = data.length > 0 ? data[data.length - 1].x : Date.now();
  const windowMs = 10000;

  return {
    chart: {
      id: 'realtime',
      type: 'line',
      redrawOnWindowResize: true,
      redrawOnParentResize: true,
      animations: { enabled: true, easing: 'linear', dynamicAnimation: { speed: 1000 } },
      toolbar: { show: false },
      zoom: { enabled: false },
      background: 'transparent',
    },
    dataLabels: { enabled: false },
    stroke: { curve: 'smooth', width: 3 },
    colors: [chartColor.value],
    fill: {
      type: 'gradient',
      gradient: { shade: 'dark', type: 'vertical', opacityFrom: 0.7, opacityTo: 0.1, stops: [0, 100] },
    },
    title: {
      text: title.value,
      align: 'left',
      style: { fontSize: '16px', fontWeight: '700', color: '#c7d0e0', fontFamily: 'Rajdhani, sans-serif' },
    },
    markers: { size: 0, hover: { size: 4, sizeOffset: 3 } },
    xaxis: {
      type: 'datetime',
      min: lastX - windowMs,
      max: lastX,
      labels: {
        datetimeUTC: false,
        style: { colors: '#8a99b3' },
        datetimeFormatter: { hour: 'HH:mm', minute: 'HH:mm:ss' },
      },
      axisBorder: { show: false },
      axisTicks: { show: false },
    },
    yaxis: {
      max: 100,
      tickAmount: 5,
      forceNiceScale: true,
      labels: { style: { colors: '#8a99b3' } },
    },
    legend: { show: false },
    tooltip: { x: { format: 'HH:mm:ss' }, theme: 'dark' },
    grid: { borderColor: '#1f2a44', strokeDashArray: 4, padding: { left: 10, right: 10 } },
    annotations: {
      yaxis: [
        {
          y: kpiTarget.value,
          borderColor: chartColor.value,
          label: {
            borderColor: chartColor.value,
            style: { color: '#fff', background: chartColor.value },
            text: `KPI · ${kpiTarget.value}`,
          },
        },
      ],
    },
  };
});
</script>

<style scoped>
.box-realTimeChart {
  display: flex;
  flex-direction: column;
  height: 100%;
  min-height: 280px;
  width: 100%;
  padding: 16px;
}
</style>
