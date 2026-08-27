<script setup>
import { useConfigStore } from '@/stores/configStore'

const configStore = useConfigStore()

function handleChange() {
  configStore.toggleUnit()
}
</script>

<template>
  <div class="unit-toggler">
    <span>온도 단위</span>
    <el-switch
      :model-value="configStore.unit === 'fahrenheit'"
      active-text="℉"
      inactive-text="℃"
      @change="handleChange"
    />
  </div>
</template>

<style scoped>
.unit-toggler {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 6px 14px;
  border-radius: 999px;
  background: rgba(255, 255, 255, 0.12);
  color: #ecfeff;
  font-size: 0.88rem;
}
</style>
