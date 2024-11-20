<script setup lang="ts">
defineProps<{
  label: string;
  variant?: 'default' | 'operator' | 'function' | 'clear' | 'memory';
}>();
</script>

<template>
  <button :class="['calculator-button', variant || 'default']">
    {{ label }}
  </button>
</template>

<style scoped>
.calculator-button {
  border: none;
  color: white;
  padding: 15px;
  font-size: 18px;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-weight: 500;
}

.default {
  background-color: #34495e;
}

.operator {
  background-color: #2980b9;
}

.function {
  background-color: #16a085;
}

.memory {
  background-color: #8e44ad;
}

.clear {
  background-color: #c0392b;
}

.calculator-button:hover {
  filter: brightness(110%);
  transform: scale(0.98);
}

.calculator-button:active {
  transform: scale(0.95);
}
</style>