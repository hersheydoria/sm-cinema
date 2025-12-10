<template>
  <div class="movie-card">
    <div class="movie-poster" @click="$emit('watch-trailer', movie)">
      <img :src="movie.poster" :alt="movie.title" />
      <div class="rating">{{ movie.rating }}</div>
      <div class="play-icon">
        <svg viewBox="0 0 24 24" fill="currentColor">
          <path d="M8 5v14l11-7z" />
        </svg>
      </div>
    </div>
    <div class="movie-info">
      <h3 class="movie-title">
        <span class="movie-title-text">{{ movie.title }}</span>
        <span v-if="movie.price" class="movie-price">{{ movie.price }}</span>
      </h3>
      <button class="buy-button" @click="$emit('buy-tickets', movie)">Buy Tickets</button>
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

defineEmits(['buy-tickets', 'watch-trailer'])
</script>

<style scoped>
.movie-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  display: flex;
  flex-direction: column;
  height: 380px;
}

.movie-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
}

.movie-poster {
  position: relative;
  width: 100%;
  overflow: hidden;
  background: #f0f0f0;
  cursor: pointer;
  height: 230px;
}

.movie-poster img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.movie-poster:hover img {
  transform: scale(1.05);
}

.play-icon {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 60px;
  height: 60px;
  background-color: rgba(249, 44, 29, 0.9);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.movie-poster:hover .play-icon {
  opacity: 1;
}

.play-icon svg {
  width: 28px;
  height: 28px;
  margin-left: 4px;
}

.movie-info {
  padding: 1rem;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.movie-title {
  font-size: 0.95rem;
  font-weight: 600;
  margin: 0 0 1rem 0;
  color: #333;
  line-height: 1.3;
  min-height: 2.6rem;
}

.movie-title-text {
  display: inline-block;
}

.movie-price {
  display: inline-block;
  margin-left: 0.35rem;
  font-size: 0.85rem;
  font-weight: 700;
  color: #e63946;
}

.buy-button {
  width: 100%;
  padding: 0.75rem;
  background-color: rgb(249, 44, 29);
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
