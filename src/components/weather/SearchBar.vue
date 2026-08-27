<script setup>
const props = defineProps({
  currentQuery: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['update-query'])
</script>

<template>
  <div class="search-bar">
    <el-input
      :model-value="props.currentQuery"
      placeholder="🔍 검색할 도시 이름을 입력하세요"
      clearable
      @update:model-value="emit('update-query', $event)"
    />
  </div>
</template>

<style scoped>
.search-bar :deep(.el-input__wrapper) {
  border-radius: var(--radius-pill);
  padding: 6px 18px;
  box-shadow: 0 0 0 1px var(--color-border) inset;
}

.search-bar :deep(.el-input__wrapper.is-focus) {
  box-shadow: 0 0 0 1.5px var(--color-primary-light) inset;
}
</style>
