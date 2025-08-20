<template>
  <div class="example-card">
    <h2>Composant d'Exemple avec SCSS</h2>
    <p>Ce composant démontre l'utilisation de SCSS dans Vue.js</p>
    
    <div class="buttons-container">
      <button class="btn btn-primary" @click="increment">
        Compteur: {{ counter }}
      </button>
      <button class="btn btn-secondary" @click="reset">
        Reset
      </button>
    </div>

    <div class="features-list">
      <h3>Fonctionnalités SCSS utilisées :</h3>
      <ul>
        <li>Variables SCSS</li>
        <li>Mixins personnalisés</li>
        <li>Nesting (imbrication)</li>
        <li>Fonctions (darken, lighten)</li>
        <li>Styles scoped</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const counter = ref(0)

const increment = () => {
  counter.value++
}

const reset = () => {
  counter.value = 0
}
</script>

<style lang="scss" scoped>
// Import des variables et mixins globaux
@import '../assets/styles/main.scss';

.example-card {
  @include card-style;
  max-width: 500px;
  margin: 2em auto;

  h2 {
    color: $secondary-color;
    margin-bottom: 1em;
    font-size: 1.5em;
  }

  p {
    color: $text-light;
    margin-bottom: 1.5em;
  }
}

.buttons-container {
  @include flex-center;
  gap: 1em;
  margin-bottom: 2em;

  .btn {
    @include button-style;

    &.btn-primary {
      @include button-style($primary-color);
    }

    &.btn-secondary {
      @include button-style($accent-color);
    }
  }
}

.features-list {
  h3 {
    color: $secondary-color;
    font-size: 1.2em;
    margin-bottom: 0.5em;
  }

  ul {
    list-style: none;
    padding: 0;

    li {
      padding: 0.5em 0;
      border-bottom: 1px solid lighten($text-light, 20%);
      position: relative;
      padding-left: 1.5em;

      &:before {
        content: "✓";
        color: $primary-color;
        font-weight: bold;
        position: absolute;
        left: 0;
      }

      &:last-child {
        border-bottom: none;
      }
    }
  }
}
</style>
