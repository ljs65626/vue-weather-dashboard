<script setup>
import { useRouter } from 'vue-router'
const router = useRouter()
</script>

<template>
  <section class="not-found">
    <p class="not-found__emoji" aria-hidden="true">🌧️</p>
    <h1>404</h1>
    <h2>페이지를 찾을 수 없습니다.</h2>
    <el-button type="primary" round @click="router.push({ name: 'WeatherHome' })">
      홈으로 이동
    </el-button>
  </section>
</template>

<style scoped>
.not-found {
  padding: 60px 24px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface);
  box-shadow: var(--shadow-card);
  text-align: center;
}

.not-found__emoji {
  margin: 0 0 8px;
  font-size: 2.4rem;
}

.not-found h1 {
  margin: 0;
  font-size: 3rem;
  color: var(--color-primary-dark);
}

.not-found h2 {
  margin: 4px 0 24px;
  color: var(--color-text-muted);
  font-size: 1.05rem;
  font-weight: 400;
}
</style>
