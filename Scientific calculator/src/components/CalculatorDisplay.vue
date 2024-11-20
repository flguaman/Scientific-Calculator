<script setup lang="ts">
defineProps<{
  value: string;
  memory?: string;
}>();
</script>

<template>
  <div class="calculator-display">
    <div class="memory-indicator" v-if="memory">M</div>
    <div class="value">{{ value }}</div>
  </div>
</template>

<style scoped>
.calculator-display {
  background-color: #1a2634;
  color: white;
  padding: 20px;
  text-align: right;
  font-size: 24px;
  margin-bottom: 20px;
  border-radius: 5px;
  min-height: 30px;
  position: relative;
}

.memory-indicator {
  position: absolute;
  top: 5px;
  left: 10px;
  font-size: 14px;
  color: #42b983;
}

.value {
  font-family: 'Courier New', monospace;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>