<template>
  <div class="weather-forecast glassmorphism">
    <h3 class="forecast-title">Prévisions 5 jours</h3>
    
    <div class="forecast-grid">
      <div 
        class="forecast-card floating"
        v-for="(day, index) in forecast" 
        :key="index"
        :style="{ animationDelay: `${index * 0.1}s` }"
      >
        <div class="day-name">{{ day.day }}</div>
        <div class="weather-icon pulsing">{{ day.icon }}</div>
        <div class="temperature">{{ day.temp }}°</div>
        <div class="condition">{{ getConditionText(day.condition) }}</div>
        
        <!-- Animation météo spécifique -->
        <div class="weather-animation" :class="day.condition">
          <div v-if="day.condition === 'rainy'" class="mini-rain">
            <div class="mini-drop" v-for="n in 3" :key="n"></div>
          </div>
          <div v-if="day.condition === 'sunny'" class="mini-sun"></div>
          <div v-if="day.condition === 'cloudy'" class="mini-cloud"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps({
  forecast: {
    type: Array,
    required: true
  }
})

const getConditionText = (condition) => {
  const conditions = {
    sunny: 'Ensoleillé',
    cloudy: 'Nuageux',
    rainy: 'Pluvieux',
    'partly-cloudy': 'Partiellement nuageux',
    stormy: 'Orageux',
    snowy: 'Neigeux'
  }
  return conditions[condition] || 'Variable'
}
</script>

<style lang="scss" scoped>
@import '../style/main.scss';

.weather-forecast {
  color: white;

  .forecast-title {
    font-size: 1.5rem;
    margin-bottom: 2rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  .forecast-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1.5rem;

    @media (max-width: 768px) {
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 1rem;
    }

    .forecast-card {
      background: rgba(255, 255, 255, 0.1);
      border-radius: 20px;
      padding: 2rem 1rem;
      text-align: center;
      border: 1px solid rgba(255, 255, 255, 0.2);
      transition: all 0.3s ease;
      position: relative;
      overflow: hidden;

      &:hover {
        background: rgba(255, 255, 255, 0.15);
        transform: translateY(-8px) scale(1.05);
        box-shadow: 0 15px 35px rgba(0, 0, 0, 0.3);
      }

      .day-name {
        font-size: 1.1rem;
        font-weight: 600;
        margin-bottom: 1rem;
        text-transform: capitalize;
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
      }

      .weather-icon {
        font-size: 3rem;
        margin: 1rem 0;
        filter: drop-shadow(2px 2px 4px rgba(0, 0, 0, 0.3));
      }

      .temperature {
        font-size: 2rem;
        font-weight: 300;
        margin-bottom: 0.5rem;
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
      }

      .condition {
        font-size: 0.9rem;
        opacity: 0.9;
        text-transform: capitalize;
      }

      .weather-animation {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        pointer-events: none;
        overflow: hidden;

        .mini-rain {
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;

          .mini-drop {
            position: absolute;
            width: 1px;
            height: 10px;
            background: rgba(59, 130, 246, 0.6);
            animation: miniRain 1.5s linear infinite;

            &:nth-child(1) {
              left: 30%;
              animation-delay: 0s;
            }

            &:nth-child(2) {
              left: 50%;
              animation-delay: 0.5s;
            }

            &:nth-child(3) {
              left: 70%;
              animation-delay: 1s;
            }
          }
        }

        .mini-sun {
          position: absolute;
          top: 20px;
          right: 20px;
          width: 20px;
          height: 20px;
          background: radial-gradient(circle, rgba(251, 191, 36, 0.8) 0%, transparent 70%);
          border-radius: 50%;
          animation: miniSunRotate 8s linear infinite;
        }

        .mini-cloud {
          position: absolute;
          top: 25px;
          right: 25px;
          width: 30px;
          height: 10px;
          background: rgba(255, 255, 255, 0.6);
          border-radius: 10px;
          animation: miniFloat 4s ease-in-out infinite;

          &::before {
            content: '';
            position: absolute;
            width: 15px;
            height: 15px;
            background: rgba(255, 255, 255, 0.6);
            border-radius: 50%;
            top: -8px;
            left: 5px;
          }
        }
      }

      // Effet de brillance au hover
      &::after {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
        transition: left 0.5s ease;
      }

      &:hover::after {
        left: 100%;
      }
    }
  }
}

@keyframes miniRain {
  0% { transform: translateY(-20px); opacity: 0; }
  10% { opacity: 1; }
  90% { opacity: 1; }
  100% { transform: translateY(120px); opacity: 0; }
}

@keyframes miniSunRotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}

@keyframes miniFloat {
  0%, 100% { transform: translateY(0px); }
  50% { transform: translateY(-5px); }
}

@media (max-width: 480px) {
  .weather-forecast {
    .forecast-grid {
      grid-template-columns: repeat(2, 1fr);
    }

    .forecast-card {
      padding: 1.5rem 0.8rem;

      .weather-icon {
        font-size: 2.5rem;
      }

      .temperature {
        font-size: 1.5rem;
      }
    }
  }
}
</style>
