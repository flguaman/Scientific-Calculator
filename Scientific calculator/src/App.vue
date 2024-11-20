<script setup lang="ts">
import ScientificCalculator from './components/ScientificCalculator.vue'
</script>

<template>
  <div class="app-container">
    <h1>Calculadora Científica</h1>
    <ScientificCalculator />
  </div>
</template>

<style>
.app-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
  background: linear-gradient(135deg, #1a2634 0%, #2c3e50 100%);
}

h1 {
  color: #fff;
  margin-bottom: 2rem;
  font-size: 2.5rem;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
}
</style>