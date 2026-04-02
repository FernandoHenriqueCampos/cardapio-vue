<script setup>
const props = defineProps(['stats']);

const formatPrice = (price) => {
  return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(price || 0);
};
</script>

<template>
  <div class="stats-bar glass fade-in">
    <div class="stat-item total">
      <span class="label">Total de Itens</span>
      <span class="value">{{ stats.total }}</span>
    </div>
    <div class="stat-item available">
      <span class="label">Disponíveis</span>
      <span class="value">{{ stats.available }}</span>
    </div>
    <div class="stat-item avg-price">
      <span class="label">Preço Médio</span>
      <span class="value accent">{{ formatPrice(stats.avgPrice) }}</span>
    </div>
  </div>
</template>

<style scoped>
.stats-bar {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  padding: 1.5rem;
  border-radius: 24px;
  background: var(--card-bg);
  backdrop-filter: blur(12px);
  border: 1px solid var(--card-border);
  margin-bottom: 2rem;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.stat-item {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  padding: 0.5rem;
  border-right: 1px solid var(--card-border);
}

.stat-item:last-child {
  border-right: none;
}

.label {
  font-size: 0.75rem;
  color: var(--text-secondary);
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.value {
  font-size: 1.5rem;
  font-weight: 700;
  color: var(--text-primary);
}

.value.accent {
  background: var(--primary-gradient);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

@media (max-width: 600px) {
  .stats-bar {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }
  
  .stat-item {
    border-right: none;
    border-bottom: 1px solid var(--card-border);
    padding-bottom: 1rem;
  }
  
  .stat-item:last-child {
    border-bottom: none;
    padding-bottom: 0;
  }
}
</style>
