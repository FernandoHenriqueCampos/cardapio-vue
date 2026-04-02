# 🍔 Cardápio Digital Master

Uma aplicação premium para gerenciamento de cardápios de lanchonetes, desenvolvida com **Vue 3** e **Vite**.

## 🚀 Como rodar o projeto

### ⚙️ Localmente com NPM
1. Instale as dependências:
   ```bash
   npm install
   ```
2. Inicie o servidor de desenvolvimento:
   ```bash
   npm run dev
   ```

### 🐳 Com Docker
1. Construa e suba o container:
   ```bash
   docker-compose up --build
   ```
2. Acesse pelo navegador em `http://localhost:8080`.

## 🧠 Conceitos de Vue 3 Aplicados

O projeto utiliza a **Composition API** e demonstra o domínio dos seguintes recursos:

1.  **Reatividade (`ref`)**:
    - Utilizado em `App.vue` (linha 9) para gerenciar o estado da lista de itens e a categoria ativa.
2.  **Propriedades Computadas (`computed`)**:
    - `filteredItems` (`App.vue`, linha 47): Filtra dinamicamente os itens conforme a categoria selecionada.
    - `stats` (`App.vue`, linha 52): Calcula o total de itens, quantos estão disponíveis e o preço médio dos itens filtrados em tempo real.
3.  **Hooks de Ciclo de Vida (`onMounted`)**:
    - `App.vue` (linha 13): Carrega os dados iniciais do `localStorage` ou popula com dados de exemplo ao montar o componente.
4.  **Persistência com `watch`**:
    - `App.vue` (linha 29): Observa mudanças profundas na lista de itens para salvar automaticamente no armazenamento local.
5.  **Comunicação Componente-Pai (`emit`)**:
    - `ItemForm.vue` (linha 11): Emite `add-item` ao cadastrar novo produto.
    - `MenuCard.vue` (linha 5): Emite `remove-item` ao excluir um item do cardápio.
    - `FilterBar.vue` (linha 3): Emite `update-category` ao clicar em um filtro.
6.  **Passagem de Dados via `props`**:
    - `MenuCard.vue` (linha 3): Recebe o objeto do item para exibição individual.
    - `StatsBar.vue` (linha 2): Recebe o objeto de estatísticas calculado pelo pai.
7.  **Diretivas Dinâmicas**:
    - `v-for`, `v-if`, `:class` e binding de estilos inline (`:style`) para personallizar cores por categoria.

## 📦 Estrutura de Componentes
O projeto foi dividido em 5 componentes para garantir a modularidade:
- `App.vue`: Gerencia o estado global, layout e filtragem.
- `ItemForm.vue`: Formulário de cadastro com validação simples.
- `FilterBar.vue`: Navegação de categorias com destaque visual.
- `StatsBar.vue`: Dashboard compacto com os dados do resumo.
- `MenuCard.vue`: Card interativo com efeitos de hover e estados visuais diferenciados para itens indisponíveis.

## 🎨 Design & UX
- Estética **Glassmorphism** com fundo escuro (`#0f172a`).
- Paleta de cores vibrantes com gradientes para cada categoria.
- Micro-animações de entrada (`fade-in`) e transições suaves em botões e cards.
- Responsividade total (Desktop/Mobile).
