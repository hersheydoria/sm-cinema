<template>
  <div class="movie-card">
    <div class="movie-poster">
      <img :src="movie.poster" :alt="movie.title" />
      <div class="rating">{{ movie.rating }}</div>
    </div>
    <div class="movie-info">
      <h3 class="movie-title">{{ movie.title }}</h3>
      <button class="buy-button">Buy Tickets</button>
    </div>
  </div>
</template>

<script setup>
defineProps({
  movie: {
    type: Object,
    required: true
  }
})
</script>

<style scoped>
.movie-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.movie-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.movie-poster {
  position: relative;
  width: 100%;
  padding-bottom: 150%;
  overflow: hidden;
  background: #f0f0f0;
}

.movie-poster img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.rating {
  position: absolute;
  top: 8px;
  left: 8px;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: 600;
}

.movie-info {
  padding: 1rem;
}

.movie-title {
  font-size: 0.95rem;
  font-weight: 600;
  margin: 0 0 1rem 0;
  color: #333;
  line-height: 1.3;
  min-height: 2.6rem;
}

.buy-button {
  width: 100%;
  padding: 0.75rem;
  background-color: #E63946;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  font-size: 0.9rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.buy-button:hover {
  background-color: #D62828;
}
</style>
