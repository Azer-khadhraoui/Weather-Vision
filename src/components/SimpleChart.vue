<template>
  <div class="weather-chart glassmorphism">
    <h3 class="chart-title">Température de la journée</h3>
    <div class="chart-placeholder">
      <div class="chart-bars">
        <div class="bar" v-for="(temp, index) in temperatures" :key="index" 
             :style="{ height: temp * 3 + 'px' }">
          <span class="temp-label">{{ temp }}°</span>
        </div>
      </div>
      <div class="chart-labels">
        <span v-for="label in timeLabels" :key="label">{{ label }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const temperatures = ref([18, 20, 22, 24, 22, 19])
const timeLabels = ref(['6h', '9h', '12h', '15h', '18h', '21h'])
</script>

<style lang="scss" scoped>
.glassmorphism {
  background: rgba(255, 255, 255, 0.1);
  backdrop-filter: blur(20px);
  border-radius: 20px;
  border: 1px solid rgba(255, 255, 255, 0.2);
  padding: 2rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
}

.weather-chart {
  color: white;
  height: 350px;

  .chart-title {
    font-size: 1.5rem;
    margin-bottom: 1.5rem;
    text-align: center;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  .chart-placeholder {
    height: 250px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;

    .chart-bars {
      display: flex;
      align-items: flex-end;
      justify-content: space-around;
      height: 200px;
      padding: 1rem 0;

      .bar {
        width: 30px;
        background: linear-gradient(to top, #3b82f6, #60a5fa);
        border-radius: 4px 4px 0 0;
        position: relative;
        transition: all 0.3s ease;
        animation: barGrow 1s ease-out;

        &:hover {
          transform: scaleY(1.1);
          background: linear-gradient(to top, #2563eb, #3b82f6);
        }

        .temp-label {
          position: absolute;
          top: -25px;
          left: 50%;
          transform: translateX(-50%);
          font-size: 0.8rem;
          font-weight: 600;
          white-space: nowrap;
        }
      }
    }

    .chart-labels {
      display: flex;
      justify-content: space-around;
      padding: 0 1rem;

      span {
        font-size: 0.9rem;
        opacity: 0.8;
      }
    }
  }
}

@keyframes barGrow {
  from { height: 0; }
  to { height: var(--final-height); }
}

@media (max-width: 768px) {
  .weather-chart {
    height: 300px;
    
    .chart-placeholder {
      height: 200px;
      
      .chart-bars {
        height: 150px;
      }
    }
    
    .chart-title {
      font-size: 1.2rem;
    }
  }
}
</style>
