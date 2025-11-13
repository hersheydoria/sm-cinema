<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <div class="logo">
          <div class="logo-circle">SM</div>
          <span class="logo-text">SMCINEMA</span>
        </div>
        <nav class="nav">
          <a href="#" class="nav-link">Home</a>
          <a href="#" class="nav-link">Movies</a>
          <a href="#" class="nav-link">Cinemas</a>
          <a href="#" class="nav-link">Events & Experiences</a>
          <a href="#" class="nav-link">Loyalty</a>
          <a href="#" class="nav-link">Shop</a>
        </nav>
      </div>
    </div>
  </header>
</template>

<script setup>
</script>

<style scoped>
.header {
  background-color: #E63946;
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
  color: #E63946;
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
}

.nav-link:hover {
  opacity: 0.8;
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
