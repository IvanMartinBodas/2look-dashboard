<template>
  <VEChart class="map-chart" :option="mapOptions" autoresize />
</template>

<script setup lang="ts">
import { ref, watchEffect, onMounted } from 'vue';
import VEChart from 'vue-echarts';
import * as echarts from 'echarts/core';
import { MapChart } from 'echarts/charts';
import { CanvasRenderer } from 'echarts/renderers';
import { TooltipComponent, VisualMapComponent, TitleComponent, ToolboxComponent } from 'echarts/components';
import europeMap from '@/assets/europe.geo.json';

echarts.use([MapChart, CanvasRenderer, TooltipComponent, VisualMapComponent, TitleComponent, ToolboxComponent]);

onMounted(() => {
  echarts.registerMap('europe', europeMap as any, {
    Iceland: { left: -20, top: 65, width: 12 },
  });
});

const props = withDefaults(defineProps<{
  title?: string;
  subtitle?: string;
  data: { name: string; value: number }[];
}>(), {
  title: 'Descargas por país',
  subtitle: 'KPI: presencia en ≥ 8 países',
  data: () => [],
});

const mapOptions = ref({});

watchEffect(() => {
  mapOptions.value = {
    title: {
      text: props.title,
      subtext: props.subtitle,
      left: 'left',
      textStyle: { color: '#c7d0e0', fontSize: 16, fontWeight: 'bold', fontFamily: 'Rajdhani, sans-serif' },
      subtextStyle: { color: '#8a99b3' },
    },
    tooltip: { trigger: 'item' },
    visualMap: {
      min: 0,
      max: 500,
      left: 'left',
      top: 'bottom',
      text: ['Más', 'Menos'],
      textStyle: { color: '#c7d0e0' },
      calculable: true,
      inRange: { color: ['#1e3a8a', '#3b82f6', '#4d8cff'] },
      outOfRange: { color: ['rgba(200,200,200,0.15)'] },
    },
    series: [
      {
        name: 'Descargas',
        type: 'map',
        map: 'europe',
        roam: true,
        emphasis: {
          label: { show: true, color: '#fff' },
          itemStyle: { areaColor: '#4d8cff', borderWidth: 1 },
        },
        itemStyle: {
          areaColor: 'rgba(128,128,128,0.3)',
          borderColor: 'rgba(255,255,255,0.3)',
          borderWidth: 0.5,
        },
        data: props.data,
      },
    ],
  };
});
</script>

<style scoped>
.map-chart {
  min-height: 300px;
  width: 100%;
  height: 100%;
}
</style>
