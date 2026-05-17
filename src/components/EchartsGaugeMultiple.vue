<template>
  <VEChart class="gauge-ring-chart" :option="chartOptions" autoresize />
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { use } from 'echarts/core';
import { GaugeChart } from 'echarts/charts';
import { CanvasRenderer } from 'echarts/renderers';
import { TooltipComponent, TitleComponent } from 'echarts/components';
import VEChart from 'vue-echarts';

use([GaugeChart, CanvasRenderer, TooltipComponent, TitleComponent]);

const props = withDefaults(defineProps<{
  title?: string;
  segments: { value: number; name?: string; color?: string }[];
}>(), {
  title: 'Latencia Gemini',
  segments: () => [
    { value: 50, name: 'Segmento 1', color: '#3b82f6' },
    { value: 70, name: 'Segmento 2', color: '#10b981' },
  ],
});

const chartOptions = computed(() => ({
  title: {
    text: props.title,
    padding: 5,
    left: 5,
    textStyle: {
      color: '#c7d0e0',
      fontWeight: 'bolder',
      fontSize: 16,
      fontFamily: 'Rajdhani, sans-serif',
    },
  },
  series: [{
    type: 'gauge',
    startAngle: 90,
    endAngle: -270,
    pointer: { show: false },
    progress: {
      show: true,
      overlap: false,
      roundCap: true,
      clip: false,
      itemStyle: { borderWidth: 1, borderColor: '#1e293b' },
    },
    axisLine: { lineStyle: { width: 18, color: [[1, '#1f2a44']] } },
    splitLine: { show: false },
    axisTick: { show: false },
    axisLabel: { show: false },
    title: { fontSize: 13, color: '#8a99b3' },
    detail: {
      width: 50,
      height: 14,
      fontSize: 14,
      color: 'inherit',
      borderColor: 'inherit',
      borderRadius: 20,
      borderWidth: 1,
      formatter: '{value}%',
    },
    data: props.segments.map((s, i) => {
      const totalSegments = props.segments.length;
      const verticalOffset = totalSegments > 1 ? -40 + (i * 80 / (totalSegments - 1)) : 0;
      return {
        value: s.value,
        name: s.name || `Segmento ${i + 1}`,
        title: { offsetCenter: ['0%', `${verticalOffset - 10}%`] },
        detail: { offsetCenter: ['0%', `${verticalOffset + 15}%`], color: '#ffffff' },
        itemStyle: { color: s.color ?? '#4d8cff' },
      };
    }),
  }],
}));
</script>

<style scoped>
.gauge-ring-chart {
  min-height: 300px;
  width: 100%;
  height: 100%;
}
</style>
