<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <div class="logo">
          <div class="logo-circle">SM</div>
          <span class="logo-text">SMCINEMA</span>
        </div>
        <nav class="nav">
          <button @click="$emit('navigate', 'home')" :class="{ active: currentPage === 'home' }" class="nav-link">Home</button>
          <button @click="$emit('navigate', 'cinemas')" :class="{ active: currentPage === 'cinemas' }" class="nav-link">Cinemas</button>
          <button @click="$emit('navigate', 'events')" :class="{ active: currentPage === 'events' }" class="nav-link">Events & Experiences</button>
          <button @click="$emit('navigate', 'loyalty')" :class="{ active: currentPage === 'loyalty' }" class="nav-link">Loyalty</button>
          <button @click="$emit('navigate', 'shop')" :class="{ active: currentPage === 'shop' }" class="nav-link">Shop</button>
        </nav>
      </div>
    </div>
  </header>
</template>

<script setup>
import { inject } from 'vue'

defineEmits(['navigate'])
const currentPage = inject('currentPage', { value: 'home' })
</script>

<style scoped>
.header {
  background-color: rgb(211, 46, 34);
  padding: 1rem 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  font-weight: 600;
  color: white;
  text-decoration: none;
  flex-shrink: 0;
}

.logo-circle {
  width: 40px;
  height: 40px;
  background-color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  color: red;
  font-size: 0.9rem;
}

.logo-text {
  color: white;
  font-size: 1.1rem;
  font-weight: 700;
  letter-spacing: 0.5px;
}

.nav {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  transition: opacity 0.3s ease;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 0;
  font-family: inherit;
  position: relative;
}

.nav-link:hover {
  opacity: 0.8;
}

.nav-link.active {
  font-weight: 700;
  border-bottom: 3px solid white;
  padding-bottom: 0.2rem;
}

@media (max-width: 768px) {
  .nav {
    gap: 1rem;
  }

  .nav-link {
    font-size: 0.85rem;
  }

  .header-content {
    gap: 1rem;
  }
}
</style>
