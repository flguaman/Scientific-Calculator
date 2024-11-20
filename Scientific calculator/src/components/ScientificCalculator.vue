<script setup lang="ts">
import { ref, computed } from 'vue';
import CalculatorDisplay from './CalculatorDisplay.vue';
import CalculatorButton from './CalculatorButton.vue';

const display = ref('0');
const previousValue = ref('');
const operation = ref('');
const newNumber = ref(true);
const memory = ref('');
const angleMode = ref<'DEG' | 'RAD'>('DEG');

const toRadians = (value: number) => {
  return angleMode.value === 'DEG' ? value * (Math.PI / 180) : value;
};

const appendNumber = (num: string) => {
  if (newNumber.value) {
    display.value = num;
    newNumber.value = false;
  } else {
    display.value = display.value === '0' ? num : display.value + num;
  }
};

const setOperation = (op: string) => {
  previousValue.value = display.value;
  operation.value = op;
  newNumber.value = true;
};

const calculate = () => {
  const prev = parseFloat(previousValue.value);
  const current = parseFloat(display.value);
  let result = 0;

  switch (operation.value) {
    case '+': result = prev + current; break;
    case '-': result = prev - current; break;
    case '*': result = prev * current; break;
    case '/': result = prev / current; break;
    case 'pow': result = Math.pow(prev, current); break;
    case 'root': result = Math.pow(prev, 1/current); break;
  }

  display.value = result.toString();
  newNumber.value = true;
};

const scientific = {
  sin: () => Math.sin(toRadians(parseFloat(display.value))),
  cos: () => Math.cos(toRadians(parseFloat(display.value))),
  tan: () => Math.tan(toRadians(parseFloat(display.value))),
  asin: () => Math.asin(parseFloat(display.value)),
  acos: () => Math.acos(parseFloat(display.value)),
  atan: () => Math.atan(parseFloat(display.value)),
  sqrt: () => Math.sqrt(parseFloat(display.value)),
  square: () => Math.pow(parseFloat(display.value), 2),
  cube: () => Math.pow(parseFloat(display.value), 3),
  log: () => Math.log10(parseFloat(display.value)),
  ln: () => Math.log(parseFloat(display.value)),
  exp: () => Math.exp(parseFloat(display.value)),
  fact: () => {
    const n = parseInt(display.value);
    let result = 1;
    for (let i = 2; i <= n; i++) result *= i;
    return result;
  },
  inv: () => 1 / parseFloat(display.value),
  abs: () => Math.abs(parseFloat(display.value)),
};

const memoryOperations = {
  mc: () => memory.value = '',
  mr: () => { display.value = memory.value; newNumber.value = true; },
  mplus: () => memory.value = (parseFloat(memory.value || '0') + parseFloat(display.value)).toString(),
  mminus: () => memory.value = (parseFloat(memory.value || '0') - parseFloat(display.value)).toString(),
  ms: () => memory.value = display.value,
};

const executeScientific = (func: keyof typeof scientific) => {
  display.value = scientific[func]().toString();
  newNumber.value = true;
};

const clear = () => {
  display.value = '0';
  previousValue.value = '';
  operation.value = '';
  newNumber.value = true;
};

const toggleAngleMode = () => {
  angleMode.value = angleMode.value === 'DEG' ? 'RAD' : 'DEG';
};

const changeSign = () => {
  display.value = (parseFloat(display.value) * -1).toString();
};
</script>

<template>
  <div class="calculator">
    <CalculatorDisplay :value="display" :memory="memory" />
    <div class="mode-toggle">
      <span :class="{ active: angleMode === 'DEG' }" @click="angleMode = 'DEG'">DEG</span>
      <span :class="{ active: angleMode === 'RAD' }" @click="angleMode = 'RAD'">RAD</span>
    </div>
    <div class="buttons">
      <div class="memory-buttons">
        <CalculatorButton @click="memoryOperations.mc()" label="MC" variant="memory" />
        <CalculatorButton @click="memoryOperations.mr()" label="MR" variant="memory" />
        <CalculatorButton @click="memoryOperations.mplus()" label="M+" variant="memory" />
        <CalculatorButton @click="memoryOperations.mminus()" label="M-" variant="memory" />
        <CalculatorButton @click="memoryOperations.ms()" label="MS" variant="memory" />
      </div>
      <div class="scientific-buttons">
        <CalculatorButton @click="executeScientific('sin')" label="sin" variant="function" />
        <CalculatorButton @click="executeScientific('cos')" label="cos" variant="function" />
        <CalculatorButton @click="executeScientific('tan')" label="tan" variant="function" />
        <CalculatorButton @click="executeScientific('asin')" label="sin⁻¹" variant="function" />
        <CalculatorButton @click="executeScientific('acos')" label="cos⁻¹" variant="function" />
        <CalculatorButton @click="executeScientific('atan')" label="tan⁻¹" variant="function" />
        <CalculatorButton @click="executeScientific('sqrt')" label="√" variant="function" />
        <CalculatorButton @click="executeScientific('square')" label="x²" variant="function" />
        <CalculatorButton @click="executeScientific('cube')" label="x³" variant="function" />
        <CalculatorButton @click="executeScientific('log')" label="log" variant="function" />
        <CalculatorButton @click="executeScientific('ln')" label="ln" variant="function" />
        <CalculatorButton @click="executeScientific('exp')" label="eˣ" variant="function" />
        <CalculatorButton @click="executeScientific('fact')" label="n!" variant="function" />
        <CalculatorButton @click="executeScientific('inv')" label="1/x" variant="function" />
        <CalculatorButton @click="executeScientific('abs')" label="|x|" variant="function" />
      </div>
      <div class="numeric-buttons">
        <CalculatorButton @click="appendNumber('7')" label="7" />
        <CalculatorButton @click="appendNumber('8')" label="8" />
        <CalculatorButton @click="appendNumber('9')" label="9" />
        <CalculatorButton @click="setOperation('/')" label="÷" variant="operator" />
        <CalculatorButton @click="appendNumber('4')" label="4" />
        <CalculatorButton @click="appendNumber('5')" label="5" />
        <CalculatorButton @click="appendNumber('6')" label="6" />
        <CalculatorButton @click="setOperation('*')" label="×" variant="operator" />
        <CalculatorButton @click="appendNumber('1')" label="1" />
        <CalculatorButton @click="appendNumber('2')" label="2" />
        <CalculatorButton @click="appendNumber('3')" label="3" />
        <CalculatorButton @click="setOperation('-')" label="-" variant="operator" />
        <CalculatorButton @click="appendNumber('0')" label="0" />
        <CalculatorButton @click="appendNumber('.')" label="." />
        <CalculatorButton @click="changeSign()" label="±" />
        <CalculatorButton @click="setOperation('+')" label="+" variant="operator" />
      </div>
      <div class="bottom-row">
        <CalculatorButton @click="clear()" label="C" variant="clear" />
        <CalculatorButton @click="calculate()" label="=" variant="operator" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  background-color: #2c3e50;
  border-radius: 10px;
  padding: 20px;
  width: 400px;
  margin: 0 auto;
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
}

.mode-toggle {
  display: flex;
  justify-content: center;
  margin-bottom: 15px;
  gap: 10px;
}

.mode-toggle span {
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  color: #fff;
  opacity: 0.6;
}

.mode-toggle span.active {
  background-color: #42b983;
  opacity: 1;
}

.buttons {
  display: grid;
  gap: 10px;
}

.memory-buttons {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 5px;
  margin-bottom: 10px;
}

.scientific-buttons {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 5px;
  margin-bottom: 10px;
}

.numeric-buttons {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 5px;
}

.bottom-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 5px;
  margin-top: 10px;
}
</style>