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

            <!-- Grid principal del Dashboard -->
            <ion-grid class="dashboard-grid">
                <!-- 🟢 Fila 1: 4 Columnas -->
                <ion-row class="ion-row-1">
                    <ion-col size="6" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Retención de Usuarios</h3>
                                <canvas ref="userRetentionChart"></canvas>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="6" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Actividad en Chats</h3>
                                <div ref="chatActivityChart" style="width: 100%; height: 90%;"></div>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="6" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Adopción por Servidores</h3>
                                <v-chart class="chart" :option="serverAdoptionOption" autoresize />
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="6" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Abandonos en Login</h3>
                                <div ref="loginDropoffChart" style="width: 100%; height: 90%;"></div>
                            </div>
                        </div>
                    </ion-col>
                </ion-row>

                <!-- 🔵 Fila 2: 2 Columnas -->
                <ion-row class="ion-row-2">
                    <ion-col size="12" size-lg="9">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Usuarios Activos en Tiempo Real</h3>
                                <div class="realtime-users-header">
                                    <div class="realtime-users-count">
                                        <span class="count-value">{{ activeUsers }}</span>
                                        <span class="count-label">usuarios activos</span>
                                    </div>
                                    <div class="realtime-users-trend" :class="usersTrend > 0 ? 'positive' : 'negative'">
                                        <span>{{ usersTrend > 0 ? '+' : '' }}{{ usersTrend }}%</span>
                                        <span class="trend-icon">{{ usersTrend > 0 ? '↑' : '↓' }}</span>
                                    </div>
                                </div>
                                <canvas ref="activeUsersChart"></canvas>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="12" size-lg="3">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Conversión</h3>
                                <div class="conversion-stats">
                                    <div class="conversion-item" v-for="(item, index) in conversionStats" :key="index">
                                        <div class="conversion-header">
                                            <div class="conversion-title">{{ item.name }}</div>
                                            <div class="conversion-rate">{{ item.rate }}%</div>
                                        </div>
                                        <div class="conversion-bar">
                                            <div class="conversion-progress"
                                                :style="{ width: `${item.rate}%`, backgroundColor: item.color }">
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </ion-col>
                </ion-row>

                <!-- 🟠 Fila 3: 2 Columnas -->
                <ion-row class="ion-row-3">
                    <ion-col size="12" size-lg="6">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Distribución Geográfica</h3>
                                <div ref="geoDistributionChart" style="width: 100%; height: 90%;"></div>
                            </div>
                        </div>
                    </ion-col>
                    <ion-col size="12" size-lg="6">
                        <div class="box">
                            <div class="chart-container">
                                <h3>Crecimiento de Usuarios</h3>
                                <canvas ref="userGrowthChart"></canvas>
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
import { LineChart } from 'echarts/charts';
import { LegendComponent, TooltipComponent, GridComponent } from 'echarts/components';
import VChart from 'vue-echarts';

// Registrar Chart.js
Chart.register(...registerables);

// Registrar ECharts
use([CanvasRenderer, LineChart, LegendComponent, TooltipComponent, GridComponent]);

// Referencias para los gráficos
const userRetentionChart = ref(null);
const chatActivityChart = ref(null);
const loginDropoffChart = ref(null);
const activeUsersChart = ref(null);
const geoDistributionChart = ref(null);
const userGrowthChart = ref(null);

// Datos para el gráfico de adopción por servidores (ECharts)
const serverAdoptionOption = reactive({
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'cross',
            label: {
                backgroundColor: '#6a7985'
            }
        }
    },
    legend: {
        data: ['Pequeños', 'Medianos', 'Grandes', 'Enterprise'],
        textStyle: {
            color: '#fff'
        }
    },
    grid: {
        left: '3%',
        right: '4%',
        bottom: '3%',
        containLabel: true
    },
    xAxis: [
        {
            type: 'category',
            boundaryGap: false,
            data: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
            axisLabel: {
                color: '#ccc'
            },
            axisLine: {
                lineStyle: {
                    color: 'rgba(255, 255, 255, 0.1)'
                }
            }
        }
    ],
    yAxis: [
        {
            type: 'value',
            axisLabel: {
                color: '#ccc'
            },
            axisLine: {
                lineStyle: {
                    color: 'rgba(255, 255, 255, 0.1)'
                }
            },
            splitLine: {
                lineStyle: {
                    color: 'rgba(255, 255, 255, 0.1)'
                }
            }
        }
    ],
    series: [
        {
            name: 'Pequeños',
            type: 'line',
            stack: 'Total',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [120, 132, 101, 134, 90, 230],
            itemStyle: {
                color: '#FF6384'
            }
        },
        {
            name: 'Medianos',
            type: 'line',
            stack: 'Total',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [220, 182, 191, 234, 290, 330],
            itemStyle: {
                color: '#36A2EB'
            }
        },
        {
            name: 'Grandes',
            type: 'line',
            stack: 'Total',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [150, 232, 201, 154, 190, 330],
            itemStyle: {
                color: '#FFCE56'
            }
        },
        {
            name: 'Enterprise',
            type: 'line',
            stack: 'Total',
            areaStyle: {},
            emphasis: {
                focus: 'series'
            },
            data: [320, 332, 301, 334, 390, 330],
            itemStyle: {
                color: '#4BC0C0'
            }
        }
    ]
});

// Estadísticas de usuarios activos en tiempo real
const activeUsers = ref(1254);
const usersTrend = ref(5.7);
const activeUsersHistory = reactive(Array.from({ length: 30 }, () => Math.floor(Math.random() * 500) + 1000));

// Estadísticas de conversión
const conversionStats = reactive([
    { name: 'Visitas → Registros', rate: 28, color: '#FF6384' },
    { name: 'Registros → Activos', rate: 65, color: '#36A2EB' },
    { name: 'Activos → Pagos', rate: 12, color: '#FFCE56' },
    { name: 'Retención 30 días', rate: 42, color: '#4BC0C0' }
]);

// Función para actualizar usuarios activos en tiempo real
const updateActiveUsers = () => {
    // Simular cambios en usuarios activos
    const change = Math.floor(Math.random() * 50) - 20;
    activeUsers.value = Math.max(800, activeUsers.value + change);

    // Calcular tendencia
    const oldAvg = activeUsersHistory.slice(0, 15).reduce((a, b) => a + b, 0) / 15;
    const newAvg = activeUsersHistory.slice(15).reduce((a, b) => a + b, 0) / 15;
    usersTrend.value = parseFloat(((newAvg - oldAvg) / oldAvg * 100).toFixed(1));

    // Actualizar historial
    activeUsersHistory.shift();
    activeUsersHistory.push(activeUsers.value);

    // Actualizar el gráfico
    if (activeUsersChart.value) {
        const chart = Chart.getChart(activeUsersChart.value);
        if (chart) {
            chart.data.labels = Array.from({ length: 30 }, (_, i) => `${30 - i}m`);
            chart.data.datasets[0].data = [...activeUsersHistory];
            chart.update();
        }
    }
};

onMounted(() => {
    // 1. Gráfico de retención de usuarios (Chart.js)
    if (userRetentionChart.value) {
        new Chart(userRetentionChart.value, {
            type: 'line',
            data: {
                labels: ['Semana 1', 'Semana 2', 'Semana 3', 'Semana 4', 'Semana 5', 'Semana 6', 'Semana 7', 'Semana 8'],
                datasets: [
                    {
                        label: 'Cohorte Ene',
                        data: [100, 80, 70, 65, 60, 55, 52, 50],
                        borderColor: '#FF6384',
                        backgroundColor: 'rgba(255, 99, 132, 0.2)',
                        tension: 0.4
                    },
                    {
                        label: 'Cohorte Feb',
                        data: [100, 85, 75, 70, 68, 65, 62, 60],
                        borderColor: '#36A2EB',
                        backgroundColor: 'rgba(54, 162, 235, 0.2)',
                        tension: 0.4
                    },
                    {
                        label: 'Cohorte Mar',
                        data: [100, 88, 80, 75, 72, 70, 68, 65],
                        borderColor: '#4BC0C0',
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        tension: 0.4
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
                            text: 'Retención (%)',
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

    // 2. Gráfico de actividad en chats (ApexCharts)
    if (chatActivityChart.value) {
        const chatActivityOptions = {
            series: [{
                name: 'Mensajes',
                data: [44, 55, 57, 56, 61, 58, 63, 60, 66]
            }, {
                name: 'Usuarios',
                data: [76, 85, 101, 98, 87, 105, 91, 114, 94]
            }],
            chart: {
                type: 'bar',
                height: '100%',
                stacked: false,
                toolbar: {
                    show: false
                },
                foreColor: '#ccc'
            },
            plotOptions: {
                bar: {
                    horizontal: false,
                    columnWidth: '55%',
                    borderRadius: 4
                },
            },
            dataLabels: {
                enabled: false
            },
            stroke: {
                show: true,
                width: 2,
                colors: ['transparent']
            },
            xaxis: {
                categories: ['Lun', 'Mar', 'Mié', 'Jue', 'Vie', 'Sáb', 'Dom', 'Lun', 'Mar'],
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                }
            },
            yaxis: {
                title: {
                    text: 'Cantidad',
                    style: {
                        color: '#ccc'
                    }
                },
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                }
            },
            fill: {
                opacity: 1
            },
            tooltip: {
                theme: 'dark'
            },
            colors: ['#FF6384', '#36A2EB'],
            grid: {
                borderColor: 'rgba(255, 255, 255, 0.1)'
            }
        };

        const chatActivityApexChart = new ApexCharts(chatActivityChart.value, chatActivityOptions);
        chatActivityApexChart.render();
    }

    // 3. Gráfico de abandonos en login (ApexCharts - Funnel)
    if (loginDropoffChart.value) {
        const loginDropoffOptions = {
            series: [
                {
                    name: "Usuarios",
                    data: [1000, 800, 600, 400, 200]
                }
            ],
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
                    borderRadius: 0,
                    horizontal: true,
                    barHeight: '80%',
                    distributed: true,
                    dataLabels: {
                        position: 'bottom'
                    },
                }
            },
            colors: ['#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF'],
            dataLabels: {
                enabled: true,
                textAnchor: 'start',
                style: {
                    colors: ['#fff']
                },
                formatter: function (val: string, opt: { w: { globals: { labels: { [x: string]: string; }; }; }; dataPointIndex: string | number; }) {
                    return opt.w.globals.labels[opt.dataPointIndex] + ": " + val
                },
                offsetX: 0,
                dropShadow: {
                    enabled: false
                }
            },
            stroke: {
                width: 1,
                colors: ['#fff']
            },
            xaxis: {
                categories: ['Visitas', 'Formulario', 'Validación', 'Registro', 'Completado'],
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                }
            },
            yaxis: {
                labels: {
                    show: false
                }
            },
            tooltip: {
                theme: 'dark',
                x: {
                    show: false
                },
                y: {
                    title: {
                        formatter: function () {
                            return ''
                        }
                    }
                }
            }
        };

        const loginDropoffApexChart = new ApexCharts(loginDropoffChart.value, loginDropoffOptions);
        loginDropoffApexChart.render();
    }

    // 4. Gráfico de usuarios activos en tiempo real (Chart.js)
    if (activeUsersChart.value) {
        new Chart(activeUsersChart.value, {
            type: 'line',
            data: {
                labels: Array.from({ length: 30 }, (_, i) => `${30 - i}m`),
                datasets: [
                    {
                        label: 'Usuarios Activos',
                        data: activeUsersHistory,
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
                        title: {
                            display: true,
                            text: 'Usuarios',
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

    // 5. Gráfico de distribución geográfica (ApexCharts - Heatmap)
    if (geoDistributionChart.value) {
        const geoDistributionOptions = {
            series: [
                {
                    name: 'España',
                    data: generateHeatmapData(8, 40, 70)
                },
                {
                    name: 'México',
                    data: generateHeatmapData(8, 30, 60)
                },
                {
                    name: 'Colombia',
                    data: generateHeatmapData(8, 20, 50)
                },
                {
                    name: 'Argentina',
                    data: generateHeatmapData(8, 15, 40)
                },
                {
                    name: 'Chile',
                    data: generateHeatmapData(8, 10, 30)
                },
                {
                    name: 'Perú',
                    data: generateHeatmapData(8, 5, 20)
                },
                {
                    name: 'Otros',
                    data: generateHeatmapData(8, 1, 15)
                }
            ],
            chart: {
                height: '100%',
                type: 'heatmap',
                toolbar: {
                    show: false
                },
                foreColor: '#ccc'
            },
            dataLabels: {
                enabled: false
            },
            colors: ["#4BC0C0"],
            xaxis: {
                categories: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun', 'Jul', 'Ago'],
                labels: {
                    style: {
                        colors: '#ccc'
                    }
                }
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

        const geoDistributionApexChart = new ApexCharts(geoDistributionChart.value, geoDistributionOptions);
        geoDistributionApexChart.render();
    }

    // 6. Gráfico de crecimiento de usuarios (Chart.js)
    if (userGrowthChart.value) {
        new Chart(userGrowthChart.value, {
            type: 'bar',
            data: {
                labels: ['Ene', 'Feb', 'Mar', 'Abr', 'May', 'Jun'],
                datasets: [
                    {
                        label: 'Nuevos Usuarios',
                        data: [120, 150, 180, 220, 270, 300],
                        backgroundColor: 'rgba(54, 162, 235, 0.7)',
                        borderColor: '#36A2EB',
                        borderWidth: 1
                    },
                    {
                        label: 'Usuarios Acumulados',
                        type: 'line',
                        data: [120, 270, 450, 670, 940, 1240],
                        borderColor: '#FF6384',
                        backgroundColor: 'rgba(255, 99, 132, 0.2)',
                        borderWidth: 2,
                        fill: true,
                        tension: 0.4,
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
                            text: 'Nuevos Usuarios',
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
                            text: 'Total Acumulado',
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

    // Iniciar actualizaciones en tiempo real
    setInterval(updateActiveUsers, 2000);
});

// Función auxiliar para generar datos de heatmap
function generateHeatmapData(count: number, min: number, max: number) {
    return Array.from({ length: count }, () => ({
        x: '',
        y: Math.floor(Math.random() * (max - min + 1)) + min
    }));
}
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

/* Estadísticas de usuarios activos en tiempo real */
.realtime-users-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
}

.realtime-users-count {
    display: flex;
    flex-direction: column;
}

.count-value {
    font-size: 28px;
    font-weight: bold;
    color: #fff;
}

.count-label {
    font-size: 14px;
    color: #ccc;
}

.realtime-users-trend {
    display: flex;
    align-items: center;
    font-size: 18px;
    font-weight: bold;
}

.realtime-users-trend.positive {
    color: #4BC0C0;
}

.realtime-users-trend.negative {
    color: #FF6384;
}

.trend-icon {
    margin-left: 5px;
}

/* Estadísticas de conversión */
.conversion-stats {
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: space-around;
}

.conversion-item {
    margin-bottom: 15px;
}

.conversion-header {
    display: flex;
    justify-content: space-between;
    margin-bottom: 5px;
}

.conversion-title {
    font-size: 14px;
    color: #fff;
}

.conversion-rate {
    font-size: 14px;
    font-weight: bold;
    color: #fff;
}

.conversion-bar {
    height: 8px;
    background-color: rgba(255, 255, 255, 0.1);
    border-radius: 4px;
    overflow: hidden;
}

.conversion-progress {
    height: 100%;
    border-radius: 4px;
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