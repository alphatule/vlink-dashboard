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

            <!-- Grid principal del Dashboard -->
            <ion-grid class="dashboard-grid">
                <!-- 🟢 Fila 1: 3 Columnas -->
                <ion-row class="ion-row-1">
                    <ion-col size="12" size-lg="4">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Rendimiento de API</h3>
                                <canvas ref="apiPerformanceChart"></canvas>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="6" size-lg="4">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Cobertura de Tests</h3>
                                <div ref="testCoverageChart" style="width: 100%; height: 90%;"></div>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="6" size-lg="4">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Errores por Módulo</h3>
                                <v-chart class="chart" :option="errorsByModuleOption" autoresize />
                            </div>
                        </div>
                    </ion-col>
                </ion-row>

                <!-- 🔵 Fila 2: 2 Columnas -->
                <ion-row class="ion-row-2">
                    <ion-col size="12" size-md="3" push-md="9">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Estado API</h3>
                                <div class="semaphore-container">
                                    <div class="semaphore">
                                        <div class="light red" :class="{ active: apiStatus === 'critical' }"></div>
                                        <div class="light yellow" :class="{ active: apiStatus === 'warning' }"></div>
                                        <div class="light green" :class="{ active: apiStatus === 'good' }"></div>
                                    </div>
                                    <div class="semaphore-label">
                                        <span>{{ apiResponseTime }}ms</span>
                                        <span class="status-text">{{ apiStatusText }}</span>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="12" size-md="9" pull-md="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Compatibilidad Móvil (Tiempo Real)</h3>
                                <div class="realtime-stats">
                                    <div class="stat-item">
                                        <div class="stat-value">{{ mobileStats.android }}%</div>
                                        <div class="stat-label">Android</div>
                                        <div class="progress-bar">
                                            <div class="progress"
                                                :style="{ width: `${mobileStats.android}%`, backgroundColor: '#3DDC84' }">
                                            </div>
                                        </div>
                                    </div>
                                    <div class="stat-item">
                                        <div class="stat-value">{{ mobileStats.ios }}%</div>
                                        <div class="stat-label">iOS</div>
                                        <div class="progress-bar">
                                            <div class="progress"
                                                :style="{ width: `${mobileStats.ios}%`, backgroundColor: '#007AFF' }">
                                            </div>
                                        </div>
                                    </div>
                                    <div class="stat-item">
                                        <div class="stat-value">{{ mobileStats.web }}%</div>
                                        <div class="stat-label">Web</div>
                                        <div class="progress-bar">
                                            <div class="progress"
                                                :style="{ width: `${mobileStats.web}%`, backgroundColor: '#FF9500' }">
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <canvas ref="mobileCompatibilityChart"></canvas>
                            </div>
                        </div>
                    </ion-col>
                </ion-row>

                <!-- 🟠 Fila 3: 3 Columnas -->
                <ion-row class="ion-row-3">
                    <ion-col size="12" size-lg="4.5">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Rendimiento por Endpoint</h3>
                                <div ref="endpointPerformanceChart" style="width: 100%; height: 90%;"></div>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="12" size-lg="4.5">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Uso de CPU/Memoria</h3>
                                <canvas ref="resourceUsageChart"></canvas>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="12" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Seguridad</h3>
                                <div class="security-stats">
                                    <div class="security-item" v-for="(item, index) in securityStats" :key="index">
                                        <div class="security-icon" :class="item.status">
                                            <i :class="item.icon"></i>
                                        </div>
                                        <div class="security-info">
                                            <div class="security-name">{{ item.name }}</div>
                                            <div class="security-status">{{ item.statusText }}</div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </ion-col>
                </ion-row>
            </ion-grid>
            <!-- Fin del Grid principal del Dashboard -->
        </ion-content>
    </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted, reactive, computed } from 'vue';
import {
    IonButtons, IonContent, IonHeader, IonMenuButton, IonPage,
    IonTitle, IonToolbar, IonGrid, IonRow, IonCol
} from '@ionic/vue';
import { Chart, registerables } from 'chart.js';
import ApexCharts from 'apexcharts';
import { use } from 'echarts/core';
import { CanvasRenderer } from 'echarts/renderers';
import { PieChart } from 'echarts/charts';
import { LegendComponent, TooltipComponent } from 'echarts/components';
import VChart from 'vue-echarts';

// Registrar Chart.js
Chart.register(...registerables);

// Registrar ECharts
use([CanvasRenderer, PieChart, LegendComponent, TooltipComponent]);

// Referencias para los gráficos
const apiPerformanceChart = ref(null);
const testCoverageChart = ref(null);
const mobileCompatibilityChart = ref(null);
const resourceUsageChart = ref(null);
const endpointPerformanceChart = ref(null);

// Estado del semáforo API
const apiResponseTime = ref(120);
const apiStatus = computed(() => {
    if (apiResponseTime.value <= 100) return 'good';
    if (apiResponseTime.value <= 300) return 'warning';
    return 'critical';
});
const apiStatusText = computed(() => {
    if (apiStatus.value === 'good') return 'Óptimo';
    if (apiStatus.value === 'warning') return 'Atención';
    return 'Crítico';
});

// Estadísticas de compatibilidad móvil en tiempo real
const mobileStats = reactive({
    android: 78,
    ios: 92,
    web: 85
});

// Datos para el gráfico de errores por módulo (ECharts)
const errorsByModuleOption = reactive({
    tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
    },
    legend: {
        orient: 'vertical',
        left: 10,
        data: ['Frontend', 'Backend', 'Database', 'Auth', 'API'],
        textStyle: {
            color: '#fff'
        }
    },
    series: [
        {
            name: 'Errores',
            type: 'pie',
            radius: ['50%', '70%'],
            avoidLabelOverlap: false,
            itemStyle: {
                borderRadius: 10,
                borderColor: '#1E1E1E',
                borderWidth: 2
            },
            label: {
                show: false,
                position: 'center'
            },
            emphasis: {
                label: {
                    show: true,
                    fontSize: '18',
                    fontWeight: 'bold'
                }
            },
            labelLine: {
                show: false
            },
            data: [
                { value: 35, name: 'Frontend', itemStyle: { color: '#FF6384' } },
                { value: 20, name: 'Backend', itemStyle: { color: '#36A2EB' } },
                { value: 15, name: 'Database', itemStyle: { color: '#FFCE56' } },
                { value: 25, name: 'Auth', itemStyle: { color: '#4BC0C0' } },
                { value: 5, name: 'API', itemStyle: { color: '#9966FF' } }
            ]
        }
    ]
});

// Datos de seguridad
const securityStats = reactive([
    { name: 'SSL/TLS', status: 'good', statusText: 'Activo', icon: 'ion-lock-closed' },
    { name: 'Firewall', status: 'good', statusText: 'Activo', icon: 'ion-shield' },
    { name: 'Autenticación', status: 'warning', statusText: 'Revisar', icon: 'ion-key' },
    { name: 'Cifrado', status: 'good', statusText: 'Activo', icon: 'ion-code-working' },
    { name: 'Backups', status: 'critical', statusText: 'Inactivo', icon: 'ion-archive' }
]);

// Función para actualizar el semáforo API
const updateApiStatus = () => {
    // Simular cambios en el tiempo de respuesta
    apiResponseTime.value = Math.floor(Math.random() * 500) + 50;
};

// Función para actualizar estadísticas de compatibilidad móvil
const updateMobileStats = () => {
    // Simular cambios en las estadísticas
    mobileStats.android = Math.floor(Math.min(100, Math.max(60, mobileStats.android + (Math.random() * 10 - 5))));
    mobileStats.ios = Math.floor(Math.min(100, Math.max(70, mobileStats.ios + (Math.random() * 10 - 5))));
    mobileStats.web = Math.floor(Math.min(100, Math.max(65, mobileStats.web + (Math.random() * 10 - 5))));

    // Actualizar el gráfico de compatibilidad móvil
    if (mobileCompatibilityChart.value) {
        const chart = Chart.getChart(mobileCompatibilityChart.value);
        if (chart) {
            chart.data.datasets[0].data = [
                mobileStats.android,
                mobileStats.ios,
                mobileStats.web
            ];
            chart.update();
        }
    }
};

onMounted(() => {
    // 1. Gráfico de rendimiento de API (Chart.js)
    if (apiPerformanceChart.value) {
        new Chart(apiPerformanceChart.value, {
            type: 'line',
            data: {
                labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago', 'Sep', 'Oct', 'Nov', 'Dic'],
                datasets: [
                    {
                        label: 'Tiempo de respuesta (ms)',
                        data: [120, 115, 130, 125, 110, 105, 115, 120, 110, 100, 95, 90],
                        borderColor: '#36A2EB',
                        backgroundColor: 'rgba(54, 162, 235, 0.2)',
                        tension: 0.4,
                        fill: true
                    },
                    {
                        label: 'Solicitudes/min',
                        data: [300, 320, 310, 340, 360, 380, 390, 400, 410, 420, 430, 450],
                        borderColor: '#FF6384',
                        backgroundColor: 'rgba(255, 99, 132, 0.2)',
                        tension: 0.4,
                        fill: true,
                        yAxisID: 'y1'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true,
                        title: {
                            display: true,
                            text: 'Tiempo (ms)',
                            color: '#fff'
                        },
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    },
                    y1: {
                        beginAtZero: true,
                        position: 'right',
                        title: {
                            display: true,
                            text: 'Solicitudes/min',
                            color: '#fff'
                        },
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            display: false
                        }
                    },
                    x: {
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    }
                },
                plugins: {
                    legend: {
                        labels: {
                            color: '#fff'
                        }
                    }
                }
            }
        });
    }

    // 2. Gráfico de cobertura de tests (ApexCharts)
    if (testCoverageChart.value) {
        const testCoverageOptions = {
            series: [{
                name: 'Cobertura',
                data: [85, 75, 92, 65, 88, 70]
            }],
            chart: {
                type: 'bar',
                height: '100%',
                toolbar: {
                    show: false
                },
                foreColor: '#ccc'
            },
            plotOptions: {
                bar: {
                    borderRadius: 4,
                    horizontal: true,
                    barHeight: '60%',
                    distributed: true
                }
            },
            dataLabels: {
                enabled: true,
                formatter: function (val: string) {
                    return val + '%';
                },
                style: {
                    fontSize: '12px',
                    colors: ['#fff']
                }
            },
            colors: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', '#FF9F40'],
            xaxis: {
                categories: ['Core', 'UI', 'API', 'Auth', 'Utils', 'Database'],
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                },
                max: 100
            },
            yaxis: {
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                }
            },
            grid: {
                borderColor: 'rgba(255, 255, 255, 0.1)'
            },
            tooltip: {
                theme: 'dark'
            }
        };

        const testCoverageApexChart = new ApexCharts(testCoverageChart.value, testCoverageOptions);
        testCoverageApexChart.render();
    }

    // 3. Gráfico de compatibilidad móvil en tiempo real (Chart.js)
    if (mobileCompatibilityChart.value) {
        new Chart(mobileCompatibilityChart.value, {
            type: 'line',
            data: {
                labels: Array.from({ length: 20 }, (_, i) => i + 1),
                datasets: [
                    {
                        label: 'Compatibilidad',
                        data: Array.from({ length: 20 }, () => Math.floor(Math.random() * 30) + 70),
                        borderColor: '#4BC0C0',
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        tension: 0.4,
                        fill: true
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: false,
                        min: 50,
                        max: 100,
                        title: {
                            display: true,
                            text: 'Compatibilidad (%)',
                            color: '#fff'
                        },
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    },
                    x: {
                        ticks: {
                            color: '#ccc',
                            maxTicksLimit: 10
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    }
                },
                plugins: {
                    legend: {
                        labels: {
                            color: '#fff'
                        }
                    }
                }
            }
        });
    }

    // 4. Gráfico de uso de recursos (Chart.js)
    if (resourceUsageChart.value) {
        new Chart(resourceUsageChart.value, {
            type: 'line',
            data: {
                labels: ['00:00', '04:00', '08:00', '12:00', '16:00', '20:00'],
                datasets: [
                    {
                        label: 'CPU (%)',
                        data: [30, 45, 65, 70, 60, 40],
                        borderColor: '#FF6384',
                        backgroundColor: 'rgba(255, 99, 132, 0.2)',
                        tension: 0.4,
                        fill: true
                    },
                    {
                        label: 'Memoria (%)',
                        data: [50, 55, 60, 65, 70, 60],
                        borderColor: '#36A2EB',
                        backgroundColor: 'rgba(54, 162, 235, 0.2)',
                        tension: 0.4,
                        fill: true
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true,
                        max: 100,
                        title: {
                            display: true,
                            text: 'Uso (%)',
                            color: '#fff'
                        },
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    },
                    x: {
                        ticks: {
                            color: '#ccc'
                        },
                        grid: {
                            color: 'rgba(255, 255, 255, 0.1)'
                        }
                    }
                },
                plugins: {
                    legend: {
                        labels: {
                            color: '#fff'
                        }
                    }
                }
            }
        });
    }

    // 5. Gráfico de rendimiento por endpoint (ApexCharts)
    if (endpointPerformanceChart.value) {
        const endpointOptions = {
            series: [{
                name: 'Tiempo de respuesta (ms)',
                data: [120, 80, 200, 150, 90, 180, 100]
            }],
            chart: {
                type: 'radar',
                height: '100%',
                toolbar: {
                    show: false
                },
                foreColor: '#ccc',
                dropShadow: {
                    enabled: true,
                    blur: 1,
                    left: 1,
                    top: 1
                }
            },
            stroke: {
                width: 2
            },
            fill: {
                opacity: 0.2
            },
            markers: {
                size: 5,
                hover: {
                    size: 8
                }
            },
            xaxis: {
                categories: ['/users', '/auth', '/products', '/orders', '/payments', '/analytics', '/settings'],
                labels: {
                    style: {
                        colors: Array(7).fill('#ccc')
                    }
                }
            },
            yaxis: {
                show: false
            },
            colors: ['#FF6384'],
            plotOptions: {
                radar: {
                    polygons: {
                        strokeColors: 'rgba(255, 255, 255, 0.1)',
                        fill: {
                            colors: ['rgba(255, 255, 255, 0.05)', 'rgba(255, 255, 255, 0)']
                        }
                    }
                }
            }
        };

        const endpointApexChart = new ApexCharts(endpointPerformanceChart.value, endpointOptions);
        endpointApexChart.render();
    }

    // Iniciar actualizaciones en tiempo real
    setInterval(updateApiStatus, 3000);
    setInterval(updateMobileStats, 2000);
});
</script>

<style scoped>
ion-row {
    overflow: hidden;
}

ion-col {
    max-height: 100%;
    --ion-grid-column-padding: 10px;
}

/* El contenido real de cada columna */
.box {
    background: #1E1E1E;
    height: 100%;
    max-height: 100%;
    overflow: hidden;
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: start;
    padding: 15px;
}

.chart-container {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

.chart-container h3 {
    margin-top: 0;
    margin-bottom: 10px;
    font-size: 16px;
    color: #fff;
    text-align: center;
}

.chart {
    height: 90%;
    width: 100%;
}

/* Semáforo API */
.semaphore-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100%;
}

.semaphore {
    display: flex;
    flex-direction: column;
    background: #333;
    border-radius: 10px;
    padding: 10px;
    margin-bottom: 15px;
}

.light {
    width: 60px;
    height: 60px;
    border-radius: 50%;
    margin: 5px;
    opacity: 0.3;
}

.light.red {
    background-color: #FF6384;
}

.light.yellow {
    background-color: #FFCE56;
}

.light.green {
    background-color: #4BC0C0;
}

.light.active {
    opacity: 1;
    box-shadow: 0 0 15px 5px rgba(255, 255, 255, 0.3);
}

.semaphore-label {
    display: flex;
    flex-direction: column;
    align-items: center;
    font-size: 18px;
    color: #fff;
}

.status-text {
    margin-top: 5px;
    font-weight: bold;
}

/* Estadísticas de compatibilidad móvil */
.realtime-stats {
    display: flex;
    justify-content: space-between;
    margin-bottom: 15px;
}

.stat-item {
    flex: 1;
    text-align: center;
    padding: 0 10px;
}

.stat-value {
    font-size: 24px;
    font-weight: bold;
    color: #fff;
}

.stat-label {
    font-size: 14px;
    color: #ccc;
    margin-bottom: 5px;
}

.progress-bar {
    height: 8px;
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
    overflow: hidden;
}

.progress {
    height: 100%;
    border-radius: 4px;
    transition: width 0.5s ease;
}

/* Estadísticas de seguridad */
.security-stats {
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: space-around;
}

.security-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
}

.security-icon {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;
}

.security-icon.good {
    background-color: rgba(75, 192, 192, 0.2);
    color: #4BC0C0;
}

.security-icon.warning {
    background-color: rgba(255, 206, 86, 0.2);
    color: #FFCE56;
}

.security-icon.critical {
    background-color: rgba(255, 99, 132, 0.2);
    color: #FF6384;
}

.security-info {
    flex: 1;
}

.security-name {
    font-size: 14px;
    color: #fff;
}

.security-status {
    font-size: 12px;
    color: #ccc;
}

/* Aplicar altura total y por filas, solo en pantallas ≥ md */
@media (min-width: 992px) {
    ion-grid {
        height: 100%;
    }

    .ion-row-1 {
        height: 20%;
        max-height: 20%;
    }

    .ion-row-2 {
        height: 40%;
        max-height: 40%;
    }

    .ion-row-3 {
        height: 40%;
        max-height: 40%;
    }
}
</style>