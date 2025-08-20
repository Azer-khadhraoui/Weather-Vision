<template>
  <div class="weather-stats glassmorphism">
    <h3 class="stats-title">Détails météo</h3>
    
    <div class="stats-grid">
      <div class="stat-card floating" v-for="(stat, key) in formattedStats" :key="key">
        <div class="stat-icon">{{ stat.icon }}</div>
        <div class="stat-content">
          <div class="stat-value">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
        <div class="stat-progress">
          <div class="progress-bar" :style="{ width: stat.percentage + '%' }"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  stats: {
    type: Object,
    required: true
  }
})

const formattedStats = computed(() => {
  return {
    humidity: {
      icon: '💧',
      value: `${props.stats.humidity}%`,
      label: 'Humidité',
      percentage: props.stats.humidity
    },
    windSpeed: {
      icon: '💨',
      value: `${props.stats.windSpeed} km/h`,
      label: 'Vitesse du vent',
      percentage: Math.min((props.stats.windSpeed / 50) * 100, 100)
    },
    pressure: {
      icon: '🌡️',
      value: `${props.stats.pressure} hPa`,
      label: 'Pression atmosphérique',
      percentage: ((props.stats.pressure - 950) / (1050 - 950)) * 100
    },
    visibility: {
      icon: '👁️',
      value: `${props.stats.visibility} km`,
      label: 'Visibilité',
      percentage: (props.stats.visibility / 20) * 100
    },
    uvIndex: {
      icon: '☀️',
      value: props.stats.uvIndex,
      label: 'Indice UV',
      percentage: (props.stats.uvIndex / 12) * 100
    }
  }
})
</script>

<style lang="scss" scoped>
@import '../style/main.scss';

.weather-stats {
  color: white;

  .stats-title {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  .stats-grid {
    display: grid;
    gap: 1rem;

    .stat-card {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 16px;
      padding: 1.5rem;
      border: 1px solid rgba(255, 255, 255, 0.2);
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;

      &:hover {
        background: rgba(255, 255, 255, 0.15);
        transform: translateY(-3px) scale(1.02);
        box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
      }

      &:nth-child(odd) {
        animation-delay: 0.1s;
      }

      &:nth-child(even) {
        animation-delay: 0.3s;
      }

      .stat-icon {
        font-size: 2rem;
        margin-bottom: 1rem;
        filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.3));
      }

      .stat-content {
        margin-bottom: 1rem;

        .stat-value {
          font-size: 1.8rem;
          font-weight: 600;
          margin-bottom: 0.5rem;
          text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }

        .stat-label {
          font-size: 0.9rem;
          opacity: 0.8;
          text-transform: uppercase;
          letter-spacing: 0.5px;
        }
      }

      .stat-progress {
        position: relative;
        height: 4px;
        background: rgba(255, 255, 255, 0.2);
        border-radius: 2px;
        overflow: hidden;

        .progress-bar {
          height: 100%;
          background: linear-gradient(90deg, #60a5fa, #3b82f6);
          border-radius: 2px;
          transition: width 1.5s ease-out;
          position: relative;

          &::after {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.4), transparent);
            animation: shimmer 2s infinite;
          }
        }
      }

      // Effet de particules au hover
      &::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: radial-gradient(circle at 50% 50%, rgba(255, 255, 255, 0.1) 0%, transparent 70%);
        opacity: 0;
        transition: opacity 0.3s ease;
        pointer-events: none;
      }

      &:hover::before {
        opacity: 1;
      }
    }
  }
}

@keyframes shimmer {
  0% { left: -100%; }
  100% { left: 100%; }
}

@media (max-width: 768px) {
  .weather-stats {
    .stats-title {
      font-size: 1.2rem;
    }

    .stats-grid .stat-card {
      padding: 1rem;

      .stat-icon {
        font-size: 1.5rem;
      }

      .stat-content .stat-value {
        font-size: 1.4rem;
      }
    }
  }
}
</style>
