<script setup>
import { onMounted, ref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import { useTemperature } from '@/composables/useTemperature'
import { fetchWeatherDetail } from '@/services/weatherApi'

const route = useRoute()
const router = useRouter()
const cityData = ref(null)
const isLoading = ref(false)
const errorMessage = ref('')

const { displayTemp, unitSymbol } = useTemperature(() => cityData.value?.temp ?? 0)

async function loadWeatherDetail() {
  isLoading.value = true
  errorMessage.value = ''

  try {
    cityData.value = await fetchWeatherDetail(String(route.params.cityId))
    if (!cityData.value) errorMessage.value = '등록되지 않은 도시입니다.'
  } catch (error) {
    console.error(error)
    errorMessage.value = '상세 날씨 정보를 불러오지 못했습니다.'
  } finally {
    isLoading.value = false
  }
}

onMounted(loadWeatherDetail)
</script>

<template>
  <section class="detail-card">
    <h2>상세 날씨</h2>
    <p v-if="isLoading">데이터를 불러오는 중입니다...</p>
    <p v-else-if="errorMessage" class="error-message">{{ errorMessage }}</p>

    <div v-else-if="cityData">
      <h3>{{ cityData.name }}</h3>
      <p class="detail-card__temp">{{ displayTemp }}{{ unitSymbol }}</p>
      <p class="detail-card__status">{{ cityData.status }}</p>

      <dl class="detail-card__stats">
        <div>
          <dt>습도</dt>
          <dd>{{ cityData.humidity }}%</dd>
        </div>
        <div>
          <dt>풍속</dt>
          <dd>{{ cityData.wind }}m/s</dd>
        </div>
      </dl>
    </div>

    <el-button type="primary" round @click="router.push({ name: 'WeatherHome' })">
      홈으로 돌아가기
    </el-button>
  </section>
</template>

<style scoped>
.detail-card {
  padding: 28px 26px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface);
  box-shadow: var(--shadow-card);
}

.detail-card h2 {
  margin: 0 0 18px;
}

.detail-card h3 {
  margin: 0 0 4px;
  font-size: 1.3rem;
}

.detail-card__temp {
  margin: 4px 0;
  font-size: 2.4rem;
  font-weight: 700;
  color: var(--color-primary-dark);
}

.detail-card__status {
  margin: 0 0 18px;
  color: var(--color-text-muted);
}

.detail-card__stats {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
  margin: 0 0 24px;
}

.detail-card__stats > div {
  padding: 14px 16px;
  border-radius: var(--radius-md);
  background: var(--color-bg);
  text-align: center;
}

.detail-card__stats dt {
  margin: 0 0 4px;
  font-size: 0.82rem;
  color: var(--color-text-muted);
}

.detail-card__stats dd {
  margin: 0;
  font-size: 1.15rem;
  font-weight: 700;
}
</style>
