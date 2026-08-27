<script setup>
import { inject } from 'vue'

// App.vue에서 provide한 값을 중간 컴포넌트 전달 없이 직접 받습니다.
const appTitle = inject('appTitle', 'Weather Dashboard')
</script>

<template>
  <section class="about-card">
    <h2>서비스 소개</h2>
    <p>
      <strong>{{ appTitle }}</strong
      >는 Vue 3 학습을 위한 실습 프로젝트입니다.
    </p>
    <p>Vue Router, Pinia, Axios, Element Plus를 하나의 SPA에서 연결합니다.</p>
  </section>
</template>

<style scoped>
.about-card {
  padding: 28px 26px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface);
  box-shadow: var(--shadow-card);
}

.about-card h2 {
  margin: 0 0 14px;
}

.about-card p {
  margin: 0 0 10px;
  color: var(--color-text-muted);
  line-height: 1.7;
}

.about-card strong {
  color: var(--color-primary-dark);
}
</style>
