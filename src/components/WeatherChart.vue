<template>
  <div class="weather-chart glassmorphism">
    <h3 class="chart-title">Température de la journée</h3>
    <div class="chart-container">
      <canvas ref="chartCanvas"></canvas>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  Filler
} from 'chart.js'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
  Filler
)

const props = defineProps({
  chartData: {
    type: Object,
    required: true
  }
})

const chartCanvas = ref(null)
let chartInstance = null

const createChart = () => {
  if (chartInstance) {
    chartInstance.destroy()
  }

  const ctx = chartCanvas.value.getContext('2d')
  
  chartInstance = new ChartJS(ctx, {
    type: 'line',
    data: {
      labels: props.chartData.labels,
      datasets: [{
        label: props.chartData.datasets[0].label,
        data: props.chartData.datasets[0].data,
        borderColor: '#60a5fa',
        backgroundColor: 'rgba(96, 165, 250, 0.1)',
        borderWidth: 3,
        fill: true,
        tension: 0.4,
        pointRadius: 6,
        pointHoverRadius: 8,
        pointBackgroundColor: '#ffffff',
        pointBorderColor: '#60a5fa',
        pointBorderWidth: 2,
        pointHoverBackgroundColor: '#60a5fa',
        pointHoverBorderColor: '#ffffff'
      }]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: {
          display: false
        },
        tooltip: {
          backgroundColor: 'rgba(0, 0, 0, 0.8)',
          titleColor: '#ffffff',
          bodyColor: '#ffffff',
          borderColor: '#60a5fa',
          borderWidth: 1,
          cornerRadius: 8,
          displayColors: false,
          callbacks: {
            label: function(context) {
              return `${context.parsed.y}°C`
            }
          }
        }
      },
      scales: {
        x: {
          grid: {
            color: 'rgba(255, 255, 255, 0.1)',
            borderColor: 'rgba(255, 255, 255, 0.2)'
          },
          ticks: {
            color: '#ffffff',
            font: {
              size: 12,
              weight: '500'
            }
          }
        },
        y: {
          grid: {
            color: 'rgba(255, 255, 255, 0.1)',
            borderColor: 'rgba(255, 255, 255, 0.2)'
          },
          ticks: {
            color: '#ffffff',
            font: {
              size: 12,
              weight: '500'
            },
            callback: function(value) {
              return value + '°C'
            }
          }
        }
      },
      interaction: {
        intersect: false,
        mode: 'index'
      },
      animation: {
        duration: 2000,
        easing: 'easeInOutQuart'
      }
    }
  })
}

onMounted(() => {
  createChart()
})

watch(() => props.chartData, () => {
  createChart()
}, { deep: true })
</script>

<style lang="scss" scoped>
@import '../style/main.scss';

.weather-chart {
  color: white;
  height: 350px;

  .chart-title {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
    color: white;
  }

  .chart-container {
    position: relative;
    height: 280px;
    width: 100%;
    
    canvas {
      border-radius: 12px;
    }
  }
}

@media (max-width: 768px) {
  .weather-chart {
    height: 300px;
    
    .chart-container {
      height: 230px;
    }
    
    .chart-title {
      font-size: 1.2rem;
    }
  }
}
</style>
