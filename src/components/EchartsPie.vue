<template>
  <VEChart class="pie-chart" :option="options" autoresize />
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { use } from 'echarts/core';
import VEChart from 'vue-echarts';
import { PieChart } from 'echarts/charts';
import { CanvasRenderer } from 'echarts/renderers';
import { TitleComponent, TooltipComponent, LegendComponent } from 'echarts/components';

use([PieChart, CanvasRenderer, TitleComponent, TooltipComponent, LegendComponent]);

const props = withDefaults(defineProps<{
  title?: string;
  data: { value: number; name: string }[];
}>(), {
  title: 'Distribución',
  data: () => [],
});

const options = computed(() => ({
  title: {
    text: props.title,
    left: 'left',
    textStyle: {
      color: '#c7d0e0',
      fontWeight: 'bold',
      fontSize: 16,
      fontFamily: 'Rajdhani, sans-serif',
    },
  },
  tooltip: { trigger: 'item', formatter: '{b}: {c} ({d}%)' },
  legend: {
    bottom: 0,
    left: 'center',
    textStyle: { color: '#c7d0e0' },
    itemWidth: 12,
    itemHeight: 12,
  },
  color: ['#4d8cff', '#10b981', '#f59e0b', '#ec4899', '#a78bfa', '#06b6d4'],
  series: [
    {
      name: props.title,
      type: 'pie',
      radius: ['45%', '70%'],
      center: ['50%', '52%'],
      avoidLabelOverlap: true,
      itemStyle: {
        borderRadius: 8,
        borderColor: '#0f1d3b',
        borderWidth: 3,
      },
      label: {
        show: true,
        color: '#c7d0e0',
        fontSize: 12,
        formatter: '{b}\n{d}%',
      },
      labelLine: { show: true, lineStyle: { color: '#8a99b3' } },
      data: props.data,
    },
  ],
}));
</script>

<style scoped>
.pie-chart {
  width: 100%;
  height: 100%;
  min-height: 280px;
}
</style>
