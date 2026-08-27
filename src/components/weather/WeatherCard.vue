<script setup>
import { useTemperature } from '@/composables/useTemperature'

const props = defineProps({
  cityItem: { type: Object, required: true },
})

const emit = defineEmits(['select-card', 'click-detail'])

// 부모가 넘겨준 섭씨 값을 Composable에 전달합니다.
const { displayTemp, unitSymbol } = useTemperature(() => props.cityItem.temp)
</script>

<template>
  <article
    class="weather-card"
    @click="emit('select-card', `${props.cityItem.name}이 선택되었습니다.`)"
  >
    <div>
      <h3>{{ props.cityItem.name }}</h3>
      <p>현재 기온: {{ displayTemp }}{{ unitSymbol }}</p>
      <p>날씨: {{ props.cityItem.status }}</p>

      <span
        class="weather-card__temp-badge"
        :class="
          props.cityItem.temp >= 25
            ? 'weather-card__temp-badge--warm'
            : 'weather-card__temp-badge--cool'
        "
      >
        {{ props.cityItem.temp >= 25 ? '더움' : '선선함' }}
      </span>
    </div>

    <el-button type="primary" round @click.stop="emit('click-detail', props.cityItem.id)">
      상세보기
    </el-button>
  </article>
</template>

<style scoped>
.weather-card {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  margin: 0 0 14px;
  padding: 16px 20px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-surface);
  cursor: pointer;
  transition: transform 0.15s ease, box-shadow 0.15s ease, border-color 0.15s ease;
}

.weather-card:last-child {
  margin-bottom: 0;
}

.weather-card:hover {
  transform: translateY(-2px);
  border-color: var(--color-primary-light);
  box-shadow: var(--shadow-card-hover);
}

.weather-card h3 {
  margin: 0 0 6px;
  font-size: 1.05rem;
}

.weather-card p {
  margin: 2px 0;
  color: var(--color-text-muted);
  font-size: 0.92rem;
}

.weather-card__temp-badge {
  display: inline-block;
  margin-top: 6px;
  padding: 3px 12px;
  border-radius: var(--radius-pill);
  font-size: 0.78rem;
  font-weight: 700;
}

.weather-card__temp-badge--warm {
  color: #92400e;
  background: rgba(245, 158, 11, 0.16);
}

.weather-card__temp-badge--cool {
  color: #075985;
  background: rgba(56, 189, 248, 0.16);
}
</style>
