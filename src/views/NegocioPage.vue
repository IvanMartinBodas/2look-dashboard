<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🚀 Negocio</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🚀 Negocio</ion-title>
        </ion-toolbar>
      </ion-header>

      <div class="story-badge">📅 2Look · 1 mes en Play Store</div>

      <ion-grid class="dashboard-grid">
        <ion-row class="ion-row-1">
          <ion-col size="6" size-lg="3">
            <div class="box"><spark-line v-bind="kpiDescargas" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><spark-line v-bind="kpiDau" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><spark-line v-bind="kpiConversion" /></div>
          </ion-col>
          <ion-col size="6" size-lg="3">
            <div class="box"><spark-line v-bind="kpiIngresos" /></div>
          </ion-col>
        </ion-row>

        <ion-row class="ion-row-2">
          <ion-col size="12" size-lg="7">
            <div class="box">
              <ApexMixedChart :series="dataIngresosSeries" title="Ingresos, reservas y nuevos usuarios (semanal)" :labels="['Sem 1', 'Sem 2', 'Sem 3', 'Sem 4']" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="5">
            <div class="box">
              <ChartJSBar :chartData="dataReservasBarberos" title="Reservas por barbero" />
            </div>
          </ion-col>
        </ion-row>

        <ion-row class="ion-row-3">
          <ion-col size="12" size-lg="7">
            <div class="box">
              <EchartsMap :data="dataDescargasEU" title="Descargas por país" subtitle="KPI: presencia en ≥ 8 países UE" />
            </div>
          </ion-col>
          <ion-col size="12" size-lg="5">
            <div class="box">
              <EchartsPie title="Formas de cara detectadas" :data="dataFormasCara" />
            </div>
          </ion-col>
        </ion-row>
      </ion-grid>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import {
  IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar,
  IonGrid, IonRow, IonCol,
} from '@ionic/vue';

import SparkLine from '@/components/SparkLine.vue';
import ApexMixedChart from '@/components/ApexMixedChart.vue';
import ChartJSBar from '@/components/ChartJSBar.vue';
import EchartsMap from '@/components/EchartsMap.vue';
import EchartsPie from '@/components/EchartsPie.vue';

const sparkOpts = (id: string, type: 'area' | 'bar' | 'line', color = '#fff') => ({
  chart: { id, type, sparkline: { enabled: true }, dropShadow: { enabled: true, top: 1, left: 1, blur: 2, opacity: 0.5 } },
  stroke: { curve: 'smooth', width: 3 },
  colors: [color],
  tooltip: { theme: 'dark', x: { show: false }, y: { title: { formatter: () => '' } } },
});

const kpiDescargas = ref({
  title: 'DESCARGAS',
  value: '1.247',
  bgColor: 'gradient-blue',
  textColor: 'white',
  iconName: 'download-outline',
  chartOptions: sparkOpts('descargas', 'area'),
  chartSeries: [{ data: [120, 180, 250, 320, 410, 580, 740, 920, 1060, 1247] }],
});

const kpiDau = ref({
  title: 'DAU',
  value: '187',
  bgColor: 'gradient-pink',
  textColor: 'white',
  iconName: 'people-outline',
  chartOptions: sparkOpts('dau', 'line'),
  chartSeries: [{ data: [30, 55, 80, 110, 130, 145, 165, 170, 180, 187] }],
});

const kpiConversion = ref({
  title: 'CONVERSIÓN IA',
  value: '32%',
  bgColor: 'gradient-green',
  textColor: 'white',
  iconName: 'trending-up-outline',
  chartOptions: sparkOpts('conv', 'area'),
  chartSeries: [{ data: [18, 21, 22, 24, 26, 28, 29, 30, 31, 32] }],
});

const kpiIngresos = ref({
  title: 'INGRESOS',
  value: '847€',
  bgColor: 'gradient-orange',
  textColor: 'white',
  iconName: 'cash-outline',
  chartOptions: sparkOpts('ingresos', 'bar'),
  chartSeries: [{ data: [60, 90, 130, 170, 210, 290, 380, 510, 680, 847] }],
});

const dataIngresosSeries = ref([
  { name: 'Reservas', type: 'column', data: [22, 35, 48, 67] },
  { name: 'Ingresos (€)', type: 'area', data: [110, 230, 320, 187] },
  { name: 'Nuevos usuarios', type: 'line', data: [180, 270, 350, 447] },
]);

const dataReservasBarberos = ref({
  labels: ['Charles', 'Richard', 'Marc', 'Anthony', 'David'],
  datasets: [
    {
      label: 'Reservas',
      data: [54, 41, 33, 27, 17],
      backgroundColor: ['#4d8cff', '#3b82f6', '#1e40af', '#1e3a8a', '#172554'],
      borderRadius: 6,
    },
  ],
});

const dataDescargasEU = ref([
  { name: 'Spain', value: 487 },
  { name: 'France', value: 198 },
  { name: 'Italy', value: 152 },
  { name: 'Portugal', value: 134 },
  { name: 'Germany', value: 98 },
  { name: 'United Kingdom', value: 76 },
  { name: 'Netherlands', value: 54 },
  { name: 'Poland', value: 48 },
]);

const dataFormasCara = ref([
  { value: 412, name: 'Ovalada' },
  { value: 287, name: 'Cuadrada' },
  { value: 201, name: 'Redonda' },
  { value: 156, name: 'Rectangular' },
  { value: 89, name: 'Corazón' },
  { value: 52, name: 'Diamante' },
]);
</script>

<style scoped>
.story-badge {
  display: inline-block;
  padding: 6px 14px;
  background: rgba(77, 140, 255, 0.12);
  border: 1px solid rgba(77, 140, 255, 0.25);
  border-radius: 999px;
  color: #6ea8ff;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 1px;
  margin-bottom: 16px;
  font-family: 'DM Sans', sans-serif;
}

ion-row {
  overflow: hidden;
}
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
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transition: border-color .2s;
}
.box:hover {
  border-color: rgba(77, 140, 255, 0.25);
}

@media (min-width: 992px) {
  ion-grid { height: calc(100% - 40px); }
  .ion-row-1 { height: 22%; max-height: 22%; }
  .ion-row-2 { height: 40%; max-height: 40%; }
  .ion-row-3 { height: 38%; max-height: 38%; }
}

ion-content {
  --background: #060c1a;
}
ion-toolbar {
  --background: #0a142a;
  --color: #c7d0e0;
}
</style>
