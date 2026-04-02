<script setup>
import { ref, computed, onMounted, watch } from 'vue';
import ItemForm from './components/ItemForm.vue';
import MenuCard from './components/MenuCard.vue';
import FilterBar from './components/FilterBar.vue';
import StatsBar from './components/StatsBar.vue';

// State
const items = ref([]);
const activeCategory = ref('Todas');

// Load initial data or from localStorage
onMounted(() => {
  const savedItems = localStorage.getItem('cardapio-items');
  if (savedItems) {
    items.value = JSON.parse(savedItems);
  } else {
    // Initial dummy data for better first look
    items.value = [
      { id: 1, name: 'Hambúrguer Gourmet', price: 32.50, category: 'Lanche', available: true },
      { id: 2, name: 'Refrigerante 350ml', price: 7.00, category: 'Bebida', available: true },
      { id: 3, name: 'Brownie com Sorvete', price: 18.00, category: 'Sobremesa', available: false },
      { id: 4, name: 'Batata Rústica', price: 15.00, category: 'Lanche', available: true },
    ];
  }
});

// Watch updates for persistence
watch(items, (newItems) => {
  localStorage.setItem('cardapio-items', JSON.stringify(newItems));
}, { deep: true });

// Actions
const addItem = (item) => {
  items.value.push(item);
};

const removeItem = (id) => {
  items.value = items.value.filter(item => item.id !== id);
};

const updateCategory = (category) => {
  activeCategory.value = category;
};

// Computed Properties
const filteredItems = computed(() => {
  if (activeCategory.value === 'Todas') return items.value;
  return items.value.filter(item => item.category === activeCategory.value);
});

const stats = computed(() => {
  const total = items.value.length;
  const available = items.value.filter(item => item.available >= 1).length;
  
  // Calculate avg price for filtered items
  const visibleAndAvailable = filteredItems.value.filter(item => item.available >= 1);
  
  const sumPrice = visibleAndAvailable.reduce((acc, item) => acc + item.price, 0);
  const avgPrice = visibleAndAvailable.length > 0 ? (sumPrice / visibleAndAvailable.length) : 0;
  
  return { total, available, avgPrice };
});
</script>

<template>
  <div class="app-wrapper">
    <header class="hero glass">
      <div class="hero-content container">
        <div class="title-section fade-in">
          <span class="badge">Premium Menu</span>
          <h1>Cardápio Digital</h1>
          <p>Gerencie sua experiência gastronômica com facilidade.</p>
        </div>
      </div>
    </header>

    <main class="container">
      <div class="main-grid">
        <aside class="sidebar">
          <ItemForm @add-item="addItem" />
          <StatsBar :stats="stats" />
        </aside>

        <section class="content-area">
          <FilterBar :active-category="activeCategory" @update-category="updateCategory" />
          
          <div v-if="filteredItems.length > 0" class="menu-grid">
            <MenuCard 
              v-for="item in filteredItems" 
              :key="item.id" 
              :item="item"
              @remove-item="removeItem"
            />
          </div>
          
          <div v-else class="empty-state glass fade-in">
            <div class="empty-icon">🍽️</div>
            <h3>Nenhum item encontrado</h3>
            <p>Não há itens cadastrados na categoria <b>{{ activeCategory }}</b>.</p>
          </div>
        </section>
      </div>
    </main>

    <footer class="main-footer">
      <div class="container footer-content">
        <p>&copy; 2026 Cardápio Digital Master. Todos os direitos reservados.</p>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.app-wrapper {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

.hero {
  padding: 4rem 0 6rem 0;
  background: linear-gradient(to bottom, rgba(15, 23, 42, 0.4), rgba(15, 23, 42, 0.1));
  backdrop-filter: blur(4px);
  margin-bottom: -3rem;
  z-index: -1;
}

.title-section {
  text-align: center;
}

.badge {
  background: var(--primary-gradient);
  padding: 0.25rem 0.75rem;
  border-radius: 20px;
  font-size: 0.7rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: white;
  margin-bottom: 1rem;
  display: inline-block;
}

h1 {
  font-size: 3.5rem;
  font-weight: 800;
  margin-bottom: 0.5rem;
  background: linear-gradient(to right, #f8fafc, #94a3b8);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.hero p {
  font-size: 1.1rem;
  color: var(--text-secondary);
  max-width: 600px;
  margin: 0 auto;
}

.main-grid {
  display: grid;
  grid-template-columns: 350px 1fr;
  gap: 3rem;
  align-items: flex-start;
}

.menu-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
}

.empty-state {
  text-align: center;
  padding: 5rem 2rem;
  border-radius: 30px;
  background: var(--card-bg);
  border: 1px dashed var(--card-border);
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 1.5rem;
  opacity: 0.5;
}

.empty-state h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  color: var(--text-primary);
}

.empty-state p {
  color: var(--text-secondary);
}

.main-footer {
  margin-top: auto;
  padding: 3rem 0;
  border-top: 1px solid var(--card-border);
}

.footer-content {
  text-align: center;
  color: var(--text-secondary);
  font-size: 0.9rem;
}

@media (max-width: 1024px) {
  .main-grid {
    grid-template-columns: 1fr;
  }
  
  .sidebar {
    max-width: 600px;
    margin: 0 auto;
    width: 100%;
  }
}

@media (max-width: 768px) {
  h1 {
    font-size: 2.5rem;
  }
  
  .hero {
    padding: 3rem 0 4rem 0;
  }
}
</style>
