<template>
  <div class="current-weather glassmorphism">
    <div class="weather-main">
      <div class="location-info">
        <h2 class="location">{{ weatherData.location }}</h2>
        <p class="date-time">{{ currentDateTime }}</p>
      </div>
      
      <div class="temperature-display">
        <span class="temperature">{{ weatherData.temperature }}°</span>
        <div class="weather-icon pulsing">{{ weatherIcon }}</div>
      </div>
    </div>

    <div class="weather-condition">
      <h3 class="condition-text">{{ conditionText }}</h3>
      <p class="feels-like">Ressenti {{ feelsLike }}°C</p>
    </div>

    <div class="quick-stats">
      <div class="stat-item">
        <span class="stat-icon">💧</span>
        <span class="stat-value">{{ weatherData.humidity }}%</span>
        <span class="stat-label">Humidité</span>
      </div>
      <div class="stat-item">
        <span class="stat-icon">💨</span>
        <span class="stat-value">{{ weatherData.windSpeed }} km/h</span>
        <span class="stat-label">Vent</span>
      </div>
      <div class="stat-item">
        <span class="stat-icon">🌡️</span>
        <span class="stat-value">{{ weatherData.pressure }} hPa</span>
        <span class="stat-label">Pression</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from 'vue'

const props = defineProps({
  weatherData: {
    type: Object,
    required: true
  }
})

const currentDateTime = ref('')

const weatherIcon = computed(() => {
  const icons = {
    sunny: '☀️',
    cloudy: '☁️',
    rainy: '🌧️',
    'partly-cloudy': '⛅',
    stormy: '⛈️',
    snowy: '❄️'
  }
  return icons[props.weatherData.condition] || '🌤️'
})

const conditionText = computed(() => {
  const conditions = {
    sunny: 'Ensoleillé',
    cloudy: 'Nuageux',
    rainy: 'Pluvieux',
    'partly-cloudy': 'Partiellement nuageux',
    stormy: 'Orageux',
    snowy: 'Neigeux'
  }
  return conditions[props.weatherData.condition] || 'Temps variable'
})

const feelsLike = computed(() => {
  // Calcul simplifié de la température ressentie
  return Math.round(props.weatherData.temperature + (props.weatherData.humidity / 100) * 2 - 1)
})

const updateDateTime = () => {
  const now = new Date()
  const options = {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  }
  currentDateTime.value = now.toLocaleDateString('fr-FR', options)
}

onMounted(() => {
  updateDateTime()
  setInterval(updateDateTime, 60000) // Mise à jour chaque minute
})
</script>

<style lang="scss" scoped>
@import '../style/main.scss';

.current-weather {
  text-align: center;
  color: white;
  margin-bottom: 2rem;

  .weather-main {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;

    @media (max-width: 768px) {
      flex-direction: column;
      gap: 1rem;
    }
  }

  .location-info {
    text-align: left;

    @media (max-width: 768px) {
      text-align: center;
    }

    .location {
      font-size: 2rem;
      font-weight: 600;
      margin-bottom: 0.5rem;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    }

    .date-time {
      font-size: 1rem;
      opacity: 0.9;
      text-transform: capitalize;
    }
  }

  .temperature-display {
    display: flex;
    align-items: center;
    gap: 1rem;

    .temperature {
      font-size: 4rem;
      font-weight: 300;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    }

    .weather-icon {
      font-size: 3rem;
      filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.3));
    }
  }

  .weather-condition {
    margin-bottom: 2rem;
    padding: 1rem 0;
    border-top: 1px solid rgba(255, 255, 255, 0.2);
    border-bottom: 1px solid rgba(255, 255, 255, 0.2);

    .condition-text {
      font-size: 1.5rem;
      margin-bottom: 0.5rem;
      text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
    }

    .feels-like {
      font-size: 1rem;
      opacity: 0.8;
    }
  }

  .quick-stats {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;

    @media (max-width: 480px) {
      grid-template-columns: 1fr;
    }

    .stat-item {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 0.5rem;
      padding: 1rem;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 12px;
      transition: all 0.3s ease;

      &:hover {
        background: rgba(255, 255, 255, 0.2);
        transform: translateY(-2px);
      }

      .stat-icon {
        font-size: 1.5rem;
        filter: drop-shadow(1px 1px 2px rgba(0, 0, 0, 0.3));
      }

      .stat-value {
        font-size: 1.2rem;
        font-weight: 600;
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
      }

      .stat-label {
        font-size: 0.9rem;
        opacity: 0.8;
        text-transform: uppercase;
        letter-spacing: 0.5px;
      }
    }
  }
}
</style>
