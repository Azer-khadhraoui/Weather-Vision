<template>
  <div class="weather-application">
    <header class="header">
      <h1 class="title">
        <span class="icon">🌤️</span>
        Weather Vision
      </h1>
    </header>

    <main class="content">
      <div class="current-weather">
        <div class="temperature-section">
          <span class="temperature">{{ temperature }}°</span>
          <div class="info">
            <h2>{{ location }}</h2>
            <p>{{ condition }}</p>
          </div>
        </div>
        
        <div class="weather-stats">
          <div class="stat-item">
            <span class="stat-icon">💧</span>
            <span class="stat-value">65%</span>
            <span class="stat-label">Humidité</span>
          </div>
          <div class="stat-item">
            <span class="stat-icon">💨</span>
            <span class="stat-value">12 km/h</span>
            <span class="stat-label">Vent</span>
          </div>
          <div class="stat-item">
            <span class="stat-icon">🌡️</span>
            <span class="stat-value">1013 hPa</span>
            <span class="stat-label">Pression</span>
          </div>
        </div>
      </div>

      <div class="forecast-section">
        <h3>Prévisions 5 jours</h3>
        <div class="forecast-list">
          <div class="forecast-day" v-for="day in forecast" :key="day.day">
            <span class="day-name">{{ day.day }}</span>
            <span class="day-icon">{{ day.icon }}</span>
            <span class="day-temp">{{ day.temp }}°</span>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'WeatherApplication',
  data() {
    return {
      temperature: 22,
      location: 'Paris, France',
      condition: 'Ensoleillé',
      forecast: [
        { day: 'Aujourd\'hui', temp: 22, icon: '☀️' },
        { day: 'Demain', temp: 19, icon: '☁️' },
        { day: 'Jeudi', temp: 15, icon: '🌧️' },
        { day: 'Vendredi', temp: 18, icon: '⛅' },
        { day: 'Samedi', temp: 24, icon: '☀️' }
      ]
    }
  }
}
</script>

<style lang="scss">
.weather-application {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  padding: 2rem;
  color: white;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 3rem;

  .title {
    font-size: 3rem;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    margin: 0;
    animation: float 3s ease-in-out infinite;

    .icon {
      font-size: 3.5rem;
    }
  }
}

.content {
  max-width: 1000px;
  margin: 0 auto;
  display: grid;
  gap: 2rem;
}

.current-weather {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;

  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
  }

  .temperature-section {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2rem;
    margin-bottom: 2rem;

    @media (max-width: 768px) {
      flex-direction: column;
      gap: 1rem;
    }

    .temperature {
      font-size: 5rem;
      font-weight: 300;
      text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    }

    .info {
      text-align: left;

      @media (max-width: 768px) {
        text-align: center;
      }

      h2 {
        font-size: 2rem;
        margin-bottom: 0.5rem;
        text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
      }

      p {
        font-size: 1.2rem;
        opacity: 0.9;
      }
    }
  }

  .weather-stats {
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
      }

      .stat-value {
        font-size: 1.2rem;
        font-weight: 600;
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

.forecast-section {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);

  h3 {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  .forecast-list {
    display: grid;
    gap: 1rem;

    .forecast-day {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 1rem;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 12px;
      transition: all 0.3s ease;

      &:hover {
        background: rgba(255, 255, 255, 0.2);
        transform: translateX(5px);
      }

      .day-name {
        font-weight: 600;
        flex: 1;
      }

      .day-icon {
        font-size: 1.5rem;
        margin: 0 1rem;
      }

      .day-temp {
        font-size: 1.2rem;
        font-weight: 600;
      }
    }
  }
}

@keyframes float {
  0%, 100% { 
    transform: translateY(0px); 
  }
  50% { 
    transform: translateY(-20px); 
  }
}

@media (max-width: 768px) {
  .weather-application {
    padding: 1rem;
  }

  .header .title {
    font-size: 2rem;
  }

  .current-weather .temperature-section .temperature {
    font-size: 3.5rem;
  }
}
</style>
