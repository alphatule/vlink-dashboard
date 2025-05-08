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


            <h1 class="ion-padding">🚀 KPIs de Negocio</h1>
            <ion-accordion-group expand="inset" :multiple="true">
                <ion-accordion value="1">
                    <ion-item slot="header">
                        <ion-label>⏱️ Tiempo medio de respuesta de la API (ms)</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Mejorar el tiempo de respuesta de la API a < 300 ms</div>
                </ion-accordion>
                <ion-accordion value="2">
                    <ion-item slot="header">
                        <ion-label>🔐 % de endpoints con cifrado activo</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Asegurar cifrado completo de datos sensibles antes del fin de Q2</div>
                </ion-accordion>
                <ion-accordion value="3">
                    <ion-item slot="header">
                        <ion-label>⚠️ Nº de errores/bugs reportados en módulo de eventos</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Reducir errores en eventos a una tasa < 2% en beta</div>
                </ion-accordion>
                <ion-accordion value="4">
                    <ion-item slot="header">
                        <ion-label>📱 % de funcionalidades verificadas en cada plataforma (Android/iOS)</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Garantizar compatibilidad en Android/iOS</div>
                </ion-accordion>
                <ion-accordion value="5">
                    <ion-item slot="header">
                        <ion-label>🧪 % de cobertura por tests unitarios y código duplicado (DRY ratio)</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Mejorar mantenibilidad y reducir duplicación de código</div>
                </ion-accordion>
            </ion-accordion-group>
            <br>
            <h1 class="ion-padding">📈 KPIs Técnicos</h1>
            <ion-accordion-group expand="inset" :multiple="true">
                <ion-accordion value="1">
                    <ion-item slot="header">
                        <ion-label>📊 Tasa de retención a 7 días (%)</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Lograr una retención del 60% a los 7 días durante la beta</div>
                </ion-accordion>
                <ion-accordion value="2">
                    <ion-item slot="header">
                        <ion-label>💬 % de nuevos usuarios que inician conversación en los primeros 3 días</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Que el 70% de nuevos usuarios envíen un mensaje en 3 días</div>
                </ion-accordion>
                <ion-accordion value="3">
                    <ion-item slot="header">
                        <ion-label>📝 % de usuarios que envían feedback</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Obtener feedback del 30% de usuarios beta en 2 semanas</div>
                </ion-accordion>
                <ion-accordion value="4">
                    <ion-item slot="header">
                        <ion-label>🖥️ Nº de servidores activos conectados a V-LINK</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Integrar V-LINK en al menos 5 servidores de FiveM</div>
                </ion-accordion>
                <ion-accordion value="5">
                    <ion-item slot="header">
                        <ion-label>🚪 % de usuarios que abandonan sin completar login</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content">Reducir el abandono en la pantalla de inicio/login a < 15</div>
                </ion-accordion>
            </ion-accordion-group>

        </ion-content>
    </ion-page>
</template>

<script setup lang="ts">
import { IonButtons, IonContent, IonHeader, IonMenuButton, IonPage, IonTitle, IonToolbar, IonAccordionGroup, IonAccordion, IonItem, IonLabel } from '@ionic/vue';
</script>


<style scoped>


ion-accordion.accordion-collapsing ion-item[slot='header'],
  ion-accordion.accordion-collapsed ion-item[slot='header'] {
    --background: var(--ion-color-light);
    --color: var(--ion-color-light-contrast);
  }


  ion-accordion.accordion-expanding ion-item[slot='header'],
  ion-accordion.accordion-expanded ion-item[slot='header'] {
    --background: rgba(var(--ion-color-primary-rgb), 0.14);
    color: var(--ion-color-primary);
  }


</style>
