<template>
  <ion-app>
    <ion-split-pane content-id="main-content">
      <ion-menu content-id="main-content" type="overlay">
        <ion-content>
          <ion-list id="inbox-list">
            <ion-list-header>2LOOK</ion-list-header>
            <ion-note>Data Visualization · DAM</ion-note>

            <ion-menu-toggle :auto-hide="false" v-for="(p, i) in appPages" :key="i">
              <ion-item @click="selectedIndex = i" router-direction="root" :router-link="p.url" lines="none" :detail="false" class="hydrated" :class="{ selected: selectedIndex === i }">
                <ion-icon aria-hidden="true" slot="start" :ios="p.iosIcon" :md="p.mdIcon"></ion-icon>
                <ion-label>{{ p.title }}</ion-label>
              </ion-item>
            </ion-menu-toggle>
          </ion-list>
        </ion-content>
      </ion-menu>
      <ion-router-outlet id="main-content"></ion-router-outlet>
    </ion-split-pane>
  </ion-app>
</template>

<script setup lang="ts">
import {
  IonApp,
  IonContent,
  IonIcon,
  IonItem,
  IonLabel,
  IonList,
  IonListHeader,
  IonMenu,
  IonMenuToggle,
  IonNote,
  IonRouterOutlet,
  IonSplitPane,
} from '@ionic/vue';
import { ref, onMounted, watch } from 'vue';
import { useRoute } from 'vue-router';
import {
  rocketOutline,
  rocketSharp,
  pulseOutline,
  pulseSharp,
  speedometerOutline,
  speedometerSharp,
} from 'ionicons/icons';

const selectedIndex = ref(0);
const appPages = [
  {
    title: 'Negocio',
    url: '/negocio',
    iosIcon: rocketOutline,
    mdIcon: rocketSharp,
  },
  {
    title: 'Técnico',
    url: '/tecnico',
    iosIcon: pulseOutline,
    mdIcon: pulseSharp,
  },
  {
    title: 'KPIs',
    url: '/kpis',
    iosIcon: speedometerOutline,
    mdIcon: speedometerSharp,
  },
];

const route = useRoute();

const updateSelectedIndex = () => {
  const currentPath = route.path;
  const index = appPages.findIndex(page => page.url === currentPath);
  if (index !== -1) {
    selectedIndex.value = index;
  }
};

onMounted(updateSelectedIndex);
watch(route, updateSelectedIndex);
</script>

<style scoped>
ion-split-pane {
  --side-max-width: 280px;
}

ion-menu ion-content {
  --background: #060c1a;
}

ion-menu.md ion-content {
  --padding-start: 8px;
  --padding-end: 8px;
  --padding-top: 20px;
  --padding-bottom: 20px;
}

ion-menu.md ion-list {
  padding: 20px 0;
  background: transparent;
}

ion-menu.md ion-note {
  margin-bottom: 30px;
  padding-left: 10px;
  color: #8a99b3;
}

ion-menu.md ion-list-header {
  font-family: 'Rajdhani', 'DM Sans', sans-serif;
  font-size: 26px;
  font-weight: 700;
  letter-spacing: 4px;
  color: #ffffff;
  padding-left: 10px;
  min-height: 30px;
}

ion-menu.md ion-list#inbox-list {
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}

ion-menu.md ion-item {
  --padding-start: 10px;
  --padding-end: 10px;
  --background: transparent;
  --color: #c7d0e0;
  border-radius: 6px;
  margin: 2px 0;
}

ion-menu.md ion-item.selected {
  --background: rgba(77, 140, 255, 0.14);
}

ion-menu.md ion-item.selected ion-icon,
ion-menu.md ion-item.selected ion-label {
  color: #4d8cff;
}

ion-menu.md ion-item ion-icon {
  color: #8a99b3;
}

ion-menu.md ion-item ion-label {
  font-weight: 500;
  font-family: 'DM Sans', sans-serif;
}

ion-note {
  display: inline-block;
  font-size: 14px;
  color: #8a99b3;
}

ion-item.selected {
  --color: #4d8cff;
}
</style>
