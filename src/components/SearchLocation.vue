<template>
  <div class="search-location">
    <div class="search-container glassmorphism">
      <div class="search-input-wrapper">
        <span class="search-icon">🔍</span>
        <input
          type="text"
          v-model="searchQuery"
          @keyup.enter="searchLocation"
          @input="onInput"
          placeholder="Rechercher une ville..."
          class="search-input"
        />
        <button @click="searchLocation" class="search-button">
          <span class="button-text">Rechercher</span>
        </button>
      </div>
      
      <div v-if="suggestions.length > 0" class="suggestions-dropdown">
        <div
          v-for="(suggestion, index) in suggestions"
          :key="index"
          @click="selectSuggestion(suggestion)"
          class="suggestion-item"
        >
          <span class="location-icon">📍</span>
          <span class="suggestion-text">{{ suggestion }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const emit = defineEmits(['location-selected'])

const searchQuery = ref('')
const suggestions = ref([])

// Suggestions de villes (en réalité, cela viendrait d'une API)
const cityDatabase = [
  'Paris, France',
  'Lyon, France',
  'Marseille, France',
  'Toulouse, France',
  'Nice, France',
  'Nantes, France',
  'Strasbourg, France',
  'Montpellier, France',
  'Bordeaux, France',
  'Lille, France',
  'Londres, Royaume-Uni',
  'New York, États-Unis',
  'Tokyo, Japon',
  'Sydney, Australie',
  'Berlin, Allemagne',
  'Madrid, Espagne',
  'Rome, Italie',
  'Amsterdam, Pays-Bas',
  'Barcelone, Espagne',
  'Lisbonne, Portugal'
]

const onInput = () => {
  if (searchQuery.value.length > 2) {
    suggestions.value = cityDatabase.filter(city =>
      city.toLowerCase().includes(searchQuery.value.toLowerCase())
    ).slice(0, 5)
  } else {
    suggestions.value = []
  }
}

const searchLocation = () => {
  if (searchQuery.value.trim()) {
    emit('location-selected', searchQuery.value.trim())
    suggestions.value = []
    searchQuery.value = ''
  }
}

const selectSuggestion = (suggestion) => {
  searchQuery.value = suggestion
  emit('location-selected', suggestion)
  suggestions.value = []
  searchQuery.value = ''
}
</script>

<style lang="scss" scoped>
@import '../style/main.scss';

.search-location {
  position: relative;
  max-width: 500px;
  margin: 0 auto;

  .search-container {
    position: relative;
    padding: 1.5rem;

    .search-input-wrapper {
      display: flex;
      align-items: center;
      background: rgba(255, 255, 255, 0.1);
      border-radius: 50px;
      border: 1px solid rgba(255, 255, 255, 0.2);
      overflow: hidden;
      transition: all 0.3s ease;

      &:focus-within {
        background: rgba(255, 255, 255, 0.15);
        border-color: rgba(255, 255, 255, 0.4);
        box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
        transform: scale(1.02);
      }

      .search-icon {
        padding: 0 1rem;
        font-size: 1.2rem;
        color: rgba(255, 255, 255, 0.7);
      }

      .search-input {
        flex: 1;
        background: transparent;
        border: none;
        outline: none;
        padding: 1rem 0;
        color: white;
        font-size: 1rem;
        
        &::placeholder {
          color: rgba(255, 255, 255, 0.6);
        }

        &:focus {
          color: white;
        }
      }

      .search-button {
        background: linear-gradient(45deg, $sky-blue, $primary-blue);
        border: none;
        padding: 1rem 2rem;
        color: white;
        font-weight: 600;
        cursor: pointer;
        transition: all 0.3s ease;
        border-radius: 0 50px 50px 0;

        &:hover {
          background: linear-gradient(45deg, lighten($sky-blue, 10%), lighten($primary-blue, 10%));
          transform: translateX(2px);
        }

        &:active {
          transform: scale(0.98);
        }

        .button-text {
          text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }
      }
    }

    .suggestions-dropdown {
      position: absolute;
      top: 100%;
      left: 1.5rem;
      right: 1.5rem;
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(20px);
      border-radius: 16px;
      border: 1px solid rgba(255, 255, 255, 0.2);
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
      z-index: 1000;
      overflow: hidden;
      margin-top: 0.5rem;
      animation: slideDown 0.3s ease;

      .suggestion-item {
        display: flex;
        align-items: center;
        padding: 1rem 1.5rem;
        cursor: pointer;
        transition: all 0.3s ease;
        border-bottom: 1px solid rgba(255, 255, 255, 0.1);

        &:last-child {
          border-bottom: none;
        }

        &:hover {
          background: rgba(59, 130, 246, 0.1);
          transform: translateX(5px);
        }

        .location-icon {
          margin-right: 1rem;
          font-size: 1.1rem;
          color: $sky-blue;
        }

        .suggestion-text {
          color: $night-dark;
          font-weight: 500;
        }
      }
    }
  }
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (max-width: 768px) {
  .search-location {
    max-width: 100%;

    .search-container {
      padding: 1rem;

      .search-input-wrapper {
        .search-button {
          padding: 1rem 1.5rem;
          font-size: 0.9rem;
        }
      }

      .suggestions-dropdown {
        left: 1rem;
        right: 1rem;
      }
    }
  }
}
</style>
