<script setup>
import { computed } from 'vue';

const props = defineProps(['item']);
const emit = defineEmits(['remove-item']);

const formatPrice = (price) => {
  return new Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' }).format(price);
};

const categoryIcon = computed(() => {
  switch (props.item.category) {
    case 'Lanche': return '🍔';
    case 'Bebida': return '🥤';
    case 'Sobremesa': return '🍰';
    default: return '🍽️';
  }
});

const categoryColor = computed(() => {
  switch (props.item.category) {
    case 'Lanche': return 'var(--primary-gradient)';
    case 'Bebida': return 'var(--secondary-gradient)';
    case 'Sobremesa': return 'var(--accent-gradient)';
    default: return 'var(--primary-gradient)';
  }
});
</script>

<template>
  <div class="menu-card glass fade-in" :class="{ 'unavailable': !item.available }">
    <div class="card-header">
      <div class="icon-wrapper" :style="{ background: categoryColor }">
        {{ categoryIcon }}
      </div>
      <div class="header-info">
        <h3>{{ item.name }}</h3>
        <span class="category-badge" :style="{ background: categoryColor }">{{ item.category }}</span>
      </div>
    </div>
    
    <div class="card-body">
      <div class="price-info">
        <span class="label">Preço</span>
        <span class="price">{{ formatPrice(item.price) }}</span>
      </div>
      <div class="status-indicator">
        <span :class="item.available ? 'dot available' : 'dot unavailable'"></span>
        <span>{{ item.available ? 'Disponível' : 'Esgotado' }}</span>
      </div>
    </div>

    <div class="card-footer">
      <button @click="emit('remove-item', item.id)" class="btn-remove">
        <svg viewBox="0 0 24 24" width="18" height="18" stroke="currentColor" stroke-width="2" fill="none" stroke-linecap="round" stroke-linejoin="round"><polyline points="3 6 5 6 21 6"></polyline><path d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6m3 0V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"></path><line x1="10" y1="11" x2="10" y2="17"></line><line x1="14" y1="11" x2="14" y2="17"></line></svg>
        Remover Item
      </button>
    </div>
  </div>
</template>

<style scoped>
.menu-card {
  padding: 1.5rem;
  border-radius: 20px;
  background: var(--card-bg);
  backdrop-filter: blur(8px);
  border: 1px solid var(--card-border);
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  position: relative;
  overflow: hidden;
}

.menu-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
  border-color: rgba(255, 255, 255, 0.2);
}

.menu-card.unavailable {
  opacity: 0.6;
  filter: grayscale(0.5);
}

.menu-card.unavailable h3 {
  text-decoration: line-through;
}

.card-header {
  display: flex;
  align-items: flex-start;
  gap: 1rem;
}

.icon-wrapper {
  padding: 1rem;
  border-radius: 14px;
  font-size: 1.5rem;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.header-info h3 {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--text-primary);
  margin-bottom: 0.25rem;
}

.category-badge {
  padding: 0.15rem 0.6rem;
  border-radius: 30px;
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.card-body {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.price-info {
  display: flex;
  flex-direction: column;
}

.price-info .label {
  font-size: 0.8rem;
  color: var(--text-secondary);
}

.price-info .price {
  font-size: 1.4rem;
  font-weight: 700;
  color: #10b981;
}

.status-indicator {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
}

.dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.dot.available {
  background-color: var(--success);
  box-shadow: 0 0 10px rgba(16, 185, 129, 0.5);
}

.dot.unavailable {
  background-color: var(--danger);
  box-shadow: 0 0 10px rgba(239, 68, 68, 0.5);
}

.btn-remove {
  width: 100%;
  padding: 0.75rem;
  border-radius: 12px;
  border: 1px solid rgba(239, 68, 68, 0.3);
  background: rgba(239, 68, 68, 0.1);
  color: #ef4444;
  font-family: inherit;
  font-size: 0.9rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.btn-remove:hover {
  background: rgba(239, 68, 68, 0.2);
  border-color: rgba(239, 68, 68, 0.6);
  transform: scale(1.02);
}
</style>
