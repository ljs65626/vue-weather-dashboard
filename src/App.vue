<script setup>
import { provide } from 'vue'
import { RouterLink, RouterView } from 'vue-router'
import UnitToggler from '@/components/weather/UnitToggler.vue'
provide('appTitle', 'Vue Weather Dashboard')
</script>

<template>
  <div class="app-container">
    <header class="app-header">
      <div class="app-header__brand">
        <span aria-hidden="true">🌤️</span>
        <h1>오늘의 날씨</h1>
      </div>
      <nav class="app-header__nav">
        <RouterLink to="/">홈</RouterLink>
        <RouterLink to="/about">소개</RouterLink>
      </nav>
      <UnitToggler />
    </header>

    <RouterView />
  </div>
</template>

<style scoped>
.app-header {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  margin: 0 0 32px;
  padding: 22px 28px;
  border-radius: 0 0 var(--radius-lg) var(--radius-lg);
  background: linear-gradient(135deg, var(--color-primary-dark), var(--color-primary));
  color: #ecfeff;
  box-shadow: var(--shadow-card);
}

.app-header__brand {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 1.4rem;
}

.app-header__brand h1 {
  margin: 0;
  font-size: 1.5rem;
  font-weight: 700;
  color: #ffffff;
}

.app-header__nav {
  display: flex;
  gap: 6px;
  padding: 4px;
  border-radius: var(--radius-pill);
  background: rgba(255, 255, 255, 0.12);
}

.app-header__nav :deep(a) {
  padding: 8px 16px;
  border-radius: var(--radius-pill);
  color: rgba(236, 254, 255, 0.85);
  text-decoration: none;
  font-size: 0.92rem;
  font-weight: 500;
  transition: background-color 0.2s ease, color 0.2s ease;
}

.app-header__nav :deep(a:hover) {
  background: rgba(255, 255, 255, 0.14);
  color: #ffffff;
}

.app-header__nav :deep(a.router-link-exact-active) {
  background: #ffffff;
  color: var(--color-primary-dark);
}
</style>
