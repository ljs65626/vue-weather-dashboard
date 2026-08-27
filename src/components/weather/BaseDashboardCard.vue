<template>
  <section class="dashboard-card">
    <header v-if="$slots.title" class="dashboard-card__title">
      <slot name="title" />
    </header>

    <slot />
  </section>
</template>

<style scoped>
.dashboard-card {
  margin-bottom: 20px;
  padding: 22px 24px;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface);
  box-shadow: var(--shadow-card);
}

.dashboard-card__title {
  margin-bottom: 14px;
}

.dashboard-card__title :deep(h2) {
  margin: 0;
  font-size: 1.1rem;
  color: var(--color-text);
}
</style>
