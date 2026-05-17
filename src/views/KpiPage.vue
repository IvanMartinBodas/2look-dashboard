<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-menu-button color="primary"></ion-menu-button>
        </ion-buttons>
        <ion-title>🎯 KPIs</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" class="ion-padding">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">🎯 KPIs</ion-title>
        </ion-toolbar>
      </ion-header>

      <h1 class="ion-padding section-title">🚀 KPIs de Negocio</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in negocioGoals" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>

      <br />

      <h1 class="ion-padding section-title">📈 KPIs Técnicos</h1>
      <ion-accordion-group expand="inset" :multiple="true">
        <ion-accordion v-for="item in tecnicoGoals" :key="item.id" :value="item.id.toString()">
          <ion-item slot="header">
            <ion-label>{{ item.id }}. {{ item.title }}</ion-label>
          </ion-item>
          <div class="ion-padding" slot="content">
            <p>{{ item.description }}</p>
            <ion-list :inset="true">
              <ion-item v-for="(element, index) in item.smart" :key="index">
                <ion-label><b>{{ element.letter }}</b> → {{ element.content }}</ion-label>
              </ion-item>
            </ion-list>
          </div>
        </ion-accordion>
      </ion-accordion-group>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
  IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar,
  IonAccordionGroup, IonAccordion, IonItem, IonLabel, IonList,
} from '@ionic/vue';
import { ref } from 'vue';

interface SmartElement { letter: string; content: string; }
interface SmartGoal { id: number; title: string; description: string; smart: SmartElement[]; }

const negocioGoals = ref<SmartGoal[]>([
  {
    id: 1,
    title: 'Descargas en Play Store',
    description: 'Alcanzar 1.500 descargas en el primer mes tras el lanzamiento en Play Store mediante campañas en RRSS y partnerships con 5 barberías locales.',
    smart: [
      { letter: 'S', content: 'Aumentar las descargas de la app en Play Store' },
      { letter: 'M', content: '1.500 descargas' },
      { letter: 'A', content: 'Mediante campañas en RRSS y partnerships con barberías' },
      { letter: 'R', content: 'Es nuestro principal canal de adquisición' },
      { letter: 'T', content: 'Primer mes tras el lanzamiento' },
    ],
  },
  {
    id: 2,
    title: 'Usuarios Activos Diarios (DAU)',
    description: 'Conseguir 250 usuarios activos al día durante el primer trimestre mediante notificaciones de recordatorio y novedades semanales.',
    smart: [
      { letter: 'S', content: 'Aumentar usuarios activos diarios' },
      { letter: 'M', content: '250 DAU' },
      { letter: 'A', content: 'Mediante notificaciones y novedades semanales' },
      { letter: 'R', content: 'Medir el engagement real con la app' },
      { letter: 'T', content: 'Primer trimestre' },
    ],
  },
  {
    id: 3,
    title: 'Conversión IA → Reserva',
    description: 'Lograr que el 30% de los usuarios que usan el asesor de IA terminen reservando una cita en los próximos 3 meses mejorando la calidad de las recomendaciones.',
    smart: [
      { letter: 'S', content: 'Mejorar la conversión del asesor de IA' },
      { letter: 'M', content: '30% de conversión' },
      { letter: 'A', content: 'Mejorando la calidad de recomendaciones de Gemini y FLUX' },
      { letter: 'R', content: 'Es el funnel principal de monetización' },
      { letter: 'T', content: 'Próximos 3 meses' },
    ],
  },
  {
    id: 4,
    title: 'Ingresos por Stripe',
    description: 'Generar 1.000€ en ingresos procesados durante el primer mes ofreciendo servicios de barbería de entre 10€ y 20€.',
    smart: [
      { letter: 'S', content: 'Facturar a través de la pasarela de Stripe' },
      { letter: 'M', content: '1.000€ en ingresos' },
      { letter: 'A', content: 'Con servicios de 10€-20€' },
      { letter: 'R', content: 'Validar la viabilidad económica del modelo' },
      { letter: 'T', content: 'Primer mes' },
    ],
  },
  {
    id: 5,
    title: 'Presencia internacional',
    description: 'Alcanzar presencia en al menos 8 países europeos en los primeros 6 meses mediante posicionamiento en App Store Optimization.',
    smart: [
      { letter: 'S', content: 'Llegar a usuarios fuera de España' },
      { letter: 'M', content: '8 países europeos' },
      { letter: 'A', content: 'Mediante ASO y traducciones' },
      { letter: 'R', content: 'Diversifica la base de usuarios' },
      { letter: 'T', content: 'Primeros 6 meses' },
    ],
  },
]);

const tecnicoGoals = ref<SmartGoal[]>([
  {
    id: 6,
    title: 'Uptime del backend',
    description: 'Garantizar un 99% de disponibilidad del servidor de Render durante el próximo trimestre mediante el pingeo automático cada 5 minutos con UptimeRobot.',
    smart: [
      { letter: 'S', content: 'Disponibilidad del backend en Render' },
      { letter: 'M', content: '99% de uptime' },
      { letter: 'A', content: 'Con UptimeRobot pingueando cada 5 min' },
      { letter: 'R', content: 'Necesario para que la app no caiga' },
      { letter: 'T', content: 'Próximo trimestre' },
    ],
  },
  {
    id: 7,
    title: 'Carga de CPU del backend',
    description: 'Mantener la carga media de CPU por debajo del 70% durante el próximo trimestre optimizando consultas SQL y caché.',
    smart: [
      { letter: 'S', content: 'CPU media del servidor Spring Boot' },
      { letter: 'M', content: 'Menor del 70%' },
      { letter: 'A', content: 'Optimizando consultas JPA y añadiendo caché' },
      { letter: 'R', content: 'Para no saturar Render free tier' },
      { letter: 'T', content: 'Próximo trimestre' },
    ],
  },
  {
    id: 8,
    title: 'Uso de memoria',
    description: 'No superar el 70% de RAM utilizada en Render durante el próximo trimestre liberando objetos no usados y evitando fugas de memoria.',
    smart: [
      { letter: 'S', content: 'RAM consumida por el backend' },
      { letter: 'M', content: 'Menor del 70%' },
      { letter: 'A', content: 'Liberando objetos y revisando memory leaks' },
      { letter: 'R', content: 'El free tier tiene 512MB' },
      { letter: 'T', content: 'Próximo trimestre' },
    ],
  },
  {
    id: 9,
    title: 'Usuarios online concurrentes',
    description: 'Mantener una capacidad de 100 usuarios concurrentes sin degradación de rendimiento durante el próximo año aumentando el plan de Render si es necesario.',
    smart: [
      { letter: 'S', content: 'Usuarios conectados a la vez' },
      { letter: 'M', content: '100 concurrentes' },
      { letter: 'A', content: 'Subiendo plan de Render si hace falta' },
      { letter: 'R', content: 'Garantizar buena experiencia en picos' },
      { letter: 'T', content: 'Próximo año' },
    ],
  },
  {
    id: 10,
    title: 'Latencia de la API de Gemini',
    description: 'Conseguir que el 90% de las llamadas a Gemini respondan en menos de 8 segundos durante el próximo trimestre rotando entre 3 claves y 6 modelos.',
    smart: [
      { letter: 'S', content: 'Tiempo de respuesta del asesor IA' },
      { letter: 'M', content: '< 8 s en el 90% de peticiones' },
      { letter: 'A', content: 'Con rotación entre 3 claves y 6 modelos' },
      { letter: 'R', content: 'Es el cuello de botella del flujo principal' },
      { letter: 'T', content: 'Próximo trimestre' },
    ],
  },
]);
</script>

<style scoped>
.section-title {
  font-family: 'Rajdhani', sans-serif;
  font-size: 28px;
  font-weight: 700;
  letter-spacing: 0.5px;
  color: #c7d0e0;
  margin: 0 0 12px;
}

ion-accordion.accordion-collapsing ion-item[slot='header'],
ion-accordion.accordion-collapsed ion-item[slot='header'] {
  --background: #0f1d3b;
  --color: #c7d0e0;
}

ion-accordion.accordion-expanding ion-item[slot='header'],
ion-accordion.accordion-expanded ion-item[slot='header'] {
  --background: rgba(77, 140, 255, 0.14);
  --color: #6ea8ff;
}

ion-content {
  --background: #060c1a;
}
ion-toolbar {
  --background: #0a142a;
  --color: #c7d0e0;
}
ion-accordion {
  --background: #0f1d3b;
}
</style>
