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
            <ion-accordion-group expand="inset" :multiple="false">
                <ion-accordion value="1">
                    <ion-item slot="header">
                        <ion-label>⏱️ Tiempo medio de respuesta de la API (ms)</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Reducir el tiempo de respuesta promedio de la API a menos de 300 ms.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Medición en milisegundos mediante métricas en tiempo real.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Optimización de consultas SQL y caché de Redis.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Mejora la experiencia de usuario y reduce abandonos.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Meta para Q3 2024.</div>
                </ion-accordion>

                <ion-accordion value="2">
                    <ion-item slot="header">
                        <ion-label>📱 Compatibilidad Móvil al 98%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Garantizar que el 98% de las funcionalidades funcionen en Android/iOS.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Pruebas automatizadas con Detox/Cypress.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Equipo QA dedicado y ciclos de testing semanales.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Asegura adopción en mercados móviles.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Lograr antes del lanzamiento oficial (Q4 2024).</div>
                </ion-accordion>

                <ion-accordion value="3">
                    <ion-item slot="header">
                        <ion-label>🚪 Reducción de abandonos en login al 10%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Disminuir la tasa de abandono en pantallas de inicio al 10%.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Segmentación por fuentes de tráfico (Google, redes sociales).</div>
                    <div class="ion-padding" slot="content"><b>A </b>Rediseño UX del flujo de registro.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Incrementa conversiones a usuarios activos.</div>
                    <div class="ion-padding" slot="content"><b>T </b>6 meses tras implementación.</div>
                </ion-accordion>

                <ion-accordion value="4">
                    <ion-item slot="header">
                        <ion-label>📈 Crecimiento Mensual de Usuarios del 20%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Aumentar base de usuarios en un 20% mensual.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Seguimiento con Mixpanel/Google Analytics.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Campañas de marketing en redes y partnerships.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Expansión del mercado objetivo.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Mantener durante 2024.</div>
                </ion-accordion>

                <ion-accordion value="5">
                    <ion-item slot="header">
                        <ion-label>💬 Retención a 7 días del 60%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Lograr que el 60% de usuarios nuevos usen la app tras 7 días.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Cohortes diarias en herramientas de analytics.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Notificaciones push y onboarding guiado.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Indica valor percibido por los usuarios.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Beta cerrada (Q3 2024).</div>
                </ion-accordion>

            </ion-accordion-group>
            <br>
            <h1 class="ion-padding">📈 KPIs Técnicos</h1>
            <ion-accordion-group expand="inset" :multiple="false">
                <ion-accordion value="1">
                    <ion-item slot="header">
                        <ion-label>🔐 Cifrado del 100% en endpoints críticos</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Implementar SSL/TLS en todos los endpoints con datos sensibles.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Auditoría mensual con OWASP ZAP.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Certificados Let’s Encrypt y configuración Nginx. </div>
                    <div class="ion-padding" slot="content"><b>R </b>Mitiga riesgos de brechas de seguridad.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Q2 2024.</div>
                </ion-accordion>

                <ion-accordion value="2">
                    <ion-item slot="header">
                        <ion-label>🧪 Cobertura de tests unitarios del 85%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Aumentar cobertura de tests en código base al 85%.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Reportes de Jest/Cypress en CI/CD.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Desarrollo guiado por pruebas (TDD).</div>
                    <div class="ion-padding" slot="content"><b>R </b>Reduce bugs en producción.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Trimestral (Q3-Q4 2024).</div>
                </ion-accordion>

                <ion-accordion value="3">
                    <ion-item slot="header">
                        <ion-label>⚠️ Errores en módulo de eventos &lt; 2%</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Limitar errores en el módulo de eventos al 2% de las transacciones.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Monitoreo con Sentry/New Relic.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Refactorización de código legacy.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Mejora estabilidad del core de la app.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Durante fase beta (Q3 2024).</div>
                </ion-accordion>

                <ion-accordion value="4">
                    <ion-item slot="header">
                        <ion-label>🖥️ Uso de CPU &lt; 70% en picos</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Mantener el uso de CPU bajo 70% durante picos de tráfico.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Alertas en Grafana/Prometheus.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Escalado horizontal en AWS/Azure.</div>
                    <div class="ion-padding" slot="content"><b>R </b>Previene caídas del servicio.</div>
                    <div class="ion-padding" slot="content"><b>T </b>Continuo (con revisión trimestral).</div>
                </ion-accordion>

                <ion-accordion value="5">
                    <ion-item slot="header">
                        <ion-label>📝 Feedback del 30% de usuarios beta</ion-label>
                    </ion-item>
                    <div class="ion-padding" slot="content"><b>S </b>Recopilar feedback del 30% de usuarios durante la beta.</div>
                    <div class="ion-padding" slot="content"><b>M </b>Encuestas in-app y seguimiento de respuestas.</div>
                    <div class="ion-padding" slot="content"><b>A </b>Incentivos (acceso premium por participar).</div>
                    <div class="ion-padding" slot="content"><b>R </b>Guía mejoras priorizadas.</div>
                    <div class="ion-padding" slot="content"><b>T </b>2 semanas tras lanzamiento beta.</div>
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
