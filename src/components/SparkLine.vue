<template>
  <div class="sparkline-card" :class="bgColor">
    <div class="sparkline-content">
      <div class="sparkline-icon">
        <ion-icon :icon="resolveIcon(iconName)" :style="{ color: textColor }"></ion-icon>
      </div>
      <div class="sparkline-text">
        <div class="sparkline-title" :style="{ color: textColor }">{{ title }}</div>
        <div class="sparkline-value" :style="{ color: textColor }">{{ value }}</div>
      </div>
    </div>
    <div class="sparkline-chart">
      <VueApexCharts type="area" height="60" :options="chartOptions" :series="chartSeries" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { IonIcon } from '@ionic/vue';
import VueApexCharts from 'vue3-apexcharts';
import {
  navigateOutline, eyeOutline, peopleOutline, cashOutline,
  trendingUpOutline, downloadOutline, earthOutline, timerOutline,
  pulseOutline, alertCircleOutline
} from 'ionicons/icons';

defineProps<{
  title: string;
  value: string;
  bgColor: string;
  textColor: string;
  iconName: string;
  chartOptions: any;
  chartSeries: any;
}>();

const iconMap: Record<string, any> = {
  'navigate-outline': navigateOutline,
  'eye-outline': eyeOutline,
  'people-outline': peopleOutline,
  'cash-outline': cashOutline,
  'trending-up-outline': trendingUpOutline,
  'download-outline': downloadOutline,
  'earth-outline': earthOutline,
  'timer-outline': timerOutline,
  'pulse-outline': pulseOutline,
  'alert-circle-outline': alertCircleOutline,
};

function resolveIcon(name: string) {
  return iconMap[name] || navigateOutline;
}
</script>

<style scoped>
.sparkline-card {
  width: 100%;
  height: 100%;
  min-height: 120px;
  padding: 16px;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.25);
}

.gradient-blue {
  background: linear-gradient(135deg, #1e3a8a 0%, #3b82f6 100%);
}
.gradient-pink {
  background: linear-gradient(135deg, #831843 0%, #ec4899 100%);
}
.gradient-orange {
  background: linear-gradient(135deg, #7c2d12 0%, #f97316 100%);
}
.gradient-green {
  background: linear-gradient(135deg, #064e3b 0%, #10b981 100%);
}

.sparkline-content {
  display: flex;
  align-items: center;
  gap: 12px;
  z-index: 2;
}

.sparkline-icon {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  background: rgba(255,255,255,0.15);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 22px;
}

.sparkline-text {
  display: flex;
  flex-direction: column;
}

.sparkline-title {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 1px;
  opacity: 0.85;
  font-family: 'DM Sans', sans-serif;
}

.sparkline-value {
  font-family: 'Rajdhani', sans-serif;
  font-size: 28px;
  font-weight: 700;
  line-height: 1.1;
}

.sparkline-chart {
  margin: 0 -16px -16px;
}
</style>
