<script setup lang="ts">
import { ref, computed } from 'vue';
import { MASK_OPTIONS } from '../constants/masks';
import { isIpValid, getNetworkAddress, getAddressesCount } from '../utils/ipDetails';

const ip = ref('');
const mask = ref('255.255.255.000');
const isShowResult = ref(false);

const isValid = computed(() => isIpValid(ip.value));

function calculate() {
  if (isValid.value) {
    isShowResult.value = true;
  }
}
</script>

<template>
  <div class="calculator">
    <h2>Калькулятор подсетей</h2>
    
    <div class="form-group">
      <label>IP Адрес</label>
      <input 
        v-model="ip" 
        type="text" 
        placeholder="192.168.1.1" 
        :class="{ invalid: ip.length > 0 && !isValid }"
      />
    </div>

    <div class="form-group">
      <label>Маска подсети</label>
      <select v-model="mask">
        <option v-for="m in MASK_OPTIONS" :key="m" :value="m">{{ m }}</option>
      </select>
    </div>

    <button @click="calculate" :disabled="!isValid">Рассчитать</button>

    <div v-if="isShowResult && isValid" class="result-box">
      <p>IP: <b>{{ ip }}</b></p>
      <p>Маска: <b>{{ mask }}</b></p>
      <p>Адрес сети: <b>{{ getNetworkAddress(ip, mask) }}</b></p>
      <p>Хостов: <b>{{ getAddressesCount(mask) }}</b></p>
    </div>
  </div>
</template>

<style scoped>
.calculator {
  display: flex;
  flex-direction: column;
  gap: 15px;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 8px;
  --main-color: #42b983;
}

.form-group {
  display: flex;
  flex-direction: column;
}

input, select, button {
  padding: 8px;
  margin-top: 5px;
  font-size: 16px;
}

input.invalid {
  border-color: red;
  outline-color: red;
}

button {
  background-color: var(--main-color);
  color: white;
  border: none;
  cursor: pointer;
}

button:disabled {
  background-color: #ccc;
  cursor: not-allowed;
}

.result-box {
  margin-top: 20px;
  padding: 15px;
  background: #f9f9f9;
  border-radius: 4px;
}
</style>