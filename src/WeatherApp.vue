<template>
  <div class="weather-app">
    <header class="header">
      <h1 class="title">
        <span class="icon">🌤️</span>
        Weather Vision - Tunisie
      </h1>
    </header>

    <main class="content">
      <div class="governorate-selector">
        <label>Gouvernorats de Tunisie :</label>
        <select v-model="selectedGovernorate" @change="changeGovernorate">
          <option v-for="gov in governorates" :key="gov.code" :value="gov">
            {{ gov.name }}
          </option>
        </select>
      </div>

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
            <span class="stat-value">{{ humidity }}%</span>
            <span class="stat-label">Humidité</span>
          </div>
          <div class="stat-item">
            <span class="stat-icon">💨</span>
            <span class="stat-value">{{ windSpeed }} km/h</span>
            <span class="stat-label">Vent</span>
          </div>
          <div class="stat-item">
            <span class="stat-icon">🌡️</span>
            <span class="stat-value">{{ pressure }} hPa</span>
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
            <span class="day-temp">{{ day.temp }}</span>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'WeatherApp',
  data() {
    return {
      temperature: 22,
      location: 'Tunis, Tunisie',
      condition: 'Ensoleillé',
      humidity: 65,
      windSpeed: 12,
      pressure: 1013,
      selectedGovernorate: null,
      governorates: [
        { code: 'tunis', name: 'Tunis', lat: 36.8065, lon: 10.1815 },
        { code: 'sfax', name: 'Sfax', lat: 34.7406, lon: 10.7603 },
        { code: 'sousse', name: 'Sousse', lat: 35.8256, lon: 10.6369 },
        { code: 'kairouan', name: 'Kairouan', lat: 35.6781, lon: 10.0963 },
        { code: 'bizerte', name: 'Bizerte', lat: 37.2744, lon: 9.8739 },
        { code: 'gabes', name: 'Gabès', lat: 33.8815, lon: 10.0982 },
        { code: 'ariana', name: 'Ariana', lat: 36.8663, lon: 10.1647 },
        { code: 'gafsa', name: 'Gafsa', lat: 34.4250, lon: 8.7842 },
        { code: 'monastir', name: 'Monastir', lat: 35.7643, lon: 10.8113 },
        { code: 'nabeul', name: 'Nabeul', lat: 36.4560, lon: 10.7376 },
        { code: 'tataouine', name: 'Tataouine', lat: 32.9297, lon: 10.4518 },
        { code: 'medenine', name: 'Médenine', lat: 33.3549, lon: 10.5055 }
      ],
      forecast: []
    }
  },
  mounted() {
    this.selectedGovernorate = this.governorates[0]
    this.fetchWeatherData()
  },
  methods: {
    async fetchWeatherData() {
      if (!this.selectedGovernorate) return
      
      try {
        const { lat, lon, name } = this.selectedGovernorate
        
        const response = await axios.get(`https://api.open-meteo.com/v1/forecast?latitude=${lat}&longitude=${lon}&current=temperature_2m,relative_humidity_2m,wind_speed_10m,surface_pressure&daily=temperature_2m_max,temperature_2m_min,weather_code&timezone=Africa/Tunis&forecast_days=5`)
        
        const current = response.data.current
        const daily = response.data.daily
        
        this.temperature = Math.round(current.temperature_2m)
        this.humidity = current.relative_humidity_2m
        this.windSpeed = Math.round(current.wind_speed_10m)
        this.pressure = Math.round(current.surface_pressure)
        this.location = `${name}, Tunisie`
        this.condition = this.getWeatherCondition(current.temperature_2m)
        
        this.forecast = this.generateForecast(daily)
        
        console.log(`Données météo pour ${name}:`, response.data)
      } catch (error) {
        console.error('Erreur:', error)
        this.condition = 'Erreur de chargement'
      }
    },
    changeGovernorate() {
      this.fetchWeatherData()
    },
    generateForecast(daily) {
      const days = ['Dimanche', 'Lundi', 'Mardi', 'Mercredi', 'Jeudi', 'Vendredi', 'Samedi']
      const forecast = []
      
      for (let i = 0; i < 5; i++) {
        const date = new Date()
        date.setDate(date.getDate() + i)
        
        const dayName = i === 0 ? "Aujourd'hui" : 
                       i === 1 ? "Demain" : 
                       days[date.getDay()]
        
        const maxTemp = Math.round(daily.temperature_2m_max[i])
        const minTemp = Math.round(daily.temperature_2m_min[i])
        const weatherCode = daily.weather_code[i]
        
        forecast.push({
          day: dayName,
          temp: `${maxTemp}°/${minTemp}°`,
          icon: this.getWeatherIcon(weatherCode)
        })
      }
      
      return forecast
    },
    getWeatherIcon(code) {
      const weatherIcons = {
        0: '☀️', 1: '🌤️', 2: '⛅', 3: '☁️',
        45: '🌫️', 48: '🌫️', 51: '🌦️', 53: '🌦️', 55: '🌦️',
        61: '🌧️', 63: '🌧️', 65: '🌧️', 71: '🌨️', 73: '🌨️', 75: '🌨️',
        95: '⛈️', 96: '⛈️', 99: '⛈️'
      }
      return weatherIcons[code] || '🌤️'
    },
    getWeatherCondition(temp) {
      if (temp > 35) return 'Très chaud 🔥'
      if (temp > 30) return 'Chaud ☀️'
      if (temp > 25) return 'Agréable ☀️'
      if (temp > 20) return 'Doux ⛅'
      if (temp > 15) return 'Frais ☁️'
      return 'Froid 🌧️'
    }
  }
}
</script>

<style lang="scss">
.weather-app {
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
    font-size: 2.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1rem;
    text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
    margin: 0;
    animation: float 3s ease-in-out infinite;

    .icon {
      font-size: 3rem;
    }
  }
}

.content {
  max-width: 1000px;
  margin: 0 auto;
  display: grid;
  gap: 2rem;
}

.governorate-selector {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 15px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 1.5rem;
  text-align: center;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);

  label {
    display: block;
    font-size: 1.2rem;
    font-weight: 600;
    margin-bottom: 1rem;
    color: white;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  select {
    background: rgba(255, 255, 255, 0.2);
    border: 2px solid rgba(255, 255, 255, 0.3);
    border-radius: 10px;
    padding: 0.8rem 1.2rem;
    font-size: 1rem;
    color: white;
    min-width: 250px;
    cursor: pointer;
    transition: all 0.3s ease;

    &:focus {
      outline: none;
      border-color: rgba(255, 255, 255, 0.6);
      box-shadow: 0 0 0 3px rgba(255, 255, 255, 0.1);
    }

    &:hover {
      background: rgba(255, 255, 255, 0.25);
    }

    option {
      background: #667eea;
      color: white;
      padding: 0.5rem;
    }
  }
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
  .weather-app {
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
