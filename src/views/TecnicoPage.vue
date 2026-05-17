<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>📈 Técnico</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">📈 Técnico</ion-title>
        </ion-toolbar>
      </ion-header>

      <div class="story-badge">⚡ Métricas en tiempo real · Producción</div>

      <ion-grid class="dashboard-grid">
        <ion-row class="ion-row-1">
          <ion-col size="12" size-lg="3">
            <div class="box">
              <EchartsGauge :value="uptimeValue" title="UPTIME BACKEND" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="line" title="Carga CPU (%)" color="#10b981" :min="40" :max="80" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="4.5">
            <div class="box">
              <ChartJSLineAreaRT chartType="area" title="Uso de memoria (%)" color="#3b82f6" :min="50" :max="75" />
            </div>
          </ion-col>
        </ion-row>

        <ion-row class="ion-row-2">
          <ion-col size="12" size-md="9">
            <div class="box">
              <ApexLineRT :series="series" title="Usuarios online ahora" :kpi-target="70" color="#4d8cff" />
            </div>
          </ion-col>
          <ion-col size="12" size-md="3">
            <div class="box">
              <EchartsGaugeMultiple title="Latencia Gemini" :segments="ringSegments" />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar,
  IonGrid, IonRow, IonCol,
} from '@ionic/vue';
import { ref, onMounted, onUnmounted } from 'vue';

import ApexLineRT from '@/components/ApexLineRT.vue';
import EchartsGauge from '@/components/EchartsGauge.vue';
import EchartsGaugeMultiple from '@/components/EchartsGaugeMultiple.vue';
import ChartJSLineAreaRT from '@/components/ChartJSLineAreaRT.vue';

const UPDATE_INTERVAL = 1000;
const MAX_DATA_POINTS = 60;

let lastDate = Date.now();
let interval: ReturnType<typeof setInterval>;

const data = ref<{ x: number; y: number }[]>([]);
const series = ref([{ name: 'Usuarios', data: data.value }]);

const uptimeValue = ref(99);

const ringSegments = ref([
  { value: 78, name: 'gemini-2.5', color: '#10b981', min: 70, max: 90 },
  { value: 45, name: 'gemini-2.0', color: '#f59e0b', min: 40, max: 60 },
  { value: 22, name: 'flash-lite', color: '#3b82f6', min: 15, max: 30 },
]);

function addDataRealTime() {
  const newX = lastDate + UPDATE_INTERVAL;
  const newY = Math.floor(Math.random() * 40) + 50;
  data.value.push({ x: newX, y: newY });

  if (data.value.length > MAX_DATA_POINTS) {
    data.value = data.value.slice(-2);
    series.value = [{ name: 'Usuarios', data: data.value }];
  }
  lastDate = newX;

  uptimeValue.value = Math.floor(Math.random() * 2) + 98;

  ringSegments.value.forEach((s) => {
    s.value = Math.floor(Math.random() * (s.max - s.min + 1)) + s.min;
  });
}

onMounted(() => {
  interval = setInterval(addDataRealTime, UPDATE_INTERVAL);
});

onUnmounted(() => {
  clearInterval(interval);
});
</script>

<style scoped>
.story-badge {
  display: inline-block;
  padding: 6px 14px;
  background: rgba(16, 185, 129, 0.12);
  border: 1px solid rgba(16, 185, 129, 0.25);
  border-radius: 999px;
  color: #34d399;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 1px;
  margin-bottom: 16px;
  font-family: 'DM Sans', sans-serif;
}

ion-row { overflow: hidden; }
ion-col {
  max-height: 100%;
  --ion-grid-column-padding: 10px;
}
.box {
  background: #0f1d3b;
  border: 1px solid rgba(255, 255, 255, 0.06);
  height: 100%;
  max-height: 100%;
  overflow: hidden;
  border-radius: 12px;
  display: flex;
  justify-content: center;
  align-items: start;
  transition: border-color .2s;
}
.box:hover {
  border-color: rgba(77, 140, 255, 0.25);
}

@media (min-width: 992px) {
  ion-grid { height: calc(100% - 40px); }
  .ion-row-1 { height: 45%; max-height: 45%; }
  .ion-row-2 { height: 55%; max-height: 55%; }
}

ion-content {
  --background: #060c1a;
}
ion-toolbar {
  --background: #0a142a;
  --color: #c7d0e0;
}
</style>
