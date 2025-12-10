<template>
  <div v-if="showTrailerModal" class="modal-overlay" @click="closeTrailer">
    <div class="modal-content" @click.stop>
      <button class="close-btn" @click="closeTrailer">✕</button>
      
      <div class="modal-header">
        <h2>{{ selectedMovie?.title }}</h2>
      </div>

      <div class="trailer-container">
        <iframe
          v-if="selectedMovie?.trailer"
          :src="getEmbedUrl(selectedMovie.trailer)"
          width="100%"
          height="500"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
          frameborder="0"
        ></iframe>
        <div v-else class="no-trailer">
          Trailer not available for this movie.
        </div>
      </div>
      <div v-if="selectedMovie?.synopsis" class="movie-description">
        <h3>Synopsis</h3>
        <p>{{ selectedMovie.synopsis }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const showTrailerModal = ref(false)
const selectedMovie = ref(null)

const openTrailer = (movie) => {
  selectedMovie.value = movie
  showTrailerModal.value = true
  // Prevent body scroll when modal is open
  document.body.style.overflow = 'hidden'
}

const closeTrailer = () => {
  showTrailerModal.value = false
  selectedMovie.value = null
  // Restore body scroll
  document.body.style.overflow = 'auto'
}

const getEmbedUrl = (youtubeUrl) => {
  // Convert YouTube URL to embed URL
  let videoId = ''
  
  // Handle different YouTube URL formats
  if (youtubeUrl.includes('youtube.com/watch?v=')) {
    videoId = youtubeUrl.split('v=')[1]?.split('&')[0]
  } else if (youtubeUrl.includes('youtu.be/')) {
    videoId = youtubeUrl.split('youtu.be/')[1]?.split('?')[0]
  } else if (youtubeUrl.includes('youtube.com/embed/')) {
    videoId = youtubeUrl.split('embed/')[1]?.split('?')[0]
  } else {
    // Assume it's already a video ID
    videoId = youtubeUrl
  }
  
  return `https://www.youtube.com/embed/${videoId}?autoplay=1`
}

defineExpose({
  openTrailer,
  closeTrailer
})
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.9);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
  animation: fadeIn 0.3s ease;
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal-content {
  position: relative;
  background: #ffffff;
  color: #0b0b0b;
  border-radius: 8px;
  max-width: 900px;
  width: 90%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.25);
  animation: slideUp 0.3s ease;
}

@keyframes slideUp {
  from {
    transform: translateY(50px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  background: rgba(0, 0, 0, 0.05);
  border: none;
  color: #0b0b0b;
  font-size: 28px;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background-color 0.3s ease;
  z-index: 10;
}

.close-btn:hover {
  background: rgba(0, 0, 0, 0.1);
}

.modal-header {
  padding: 24px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.08);
}

.modal-header h2 {
  margin: 0;
  color: #0b0b0b;
  font-size: 1.5rem;
}

.trailer-container {
  padding: 24px;
  background: #f8f8f8;
}

.movie-description {
  padding: 0 24px 24px;
  border-top: 1px solid rgba(0, 0, 0, 0.08);
}

.movie-description h3 {
  margin: 16px 0 8px;
  color: #0b0b0b;
  font-size: 1.1rem;
}

.movie-description p {
  margin: 0;
  color: #1f1f1f;
  line-height: 1.5;
}

.trailer-container iframe {
  border-radius: 4px;
  display: block;
}

.no-trailer {
  color: #888;
  text-align: center;
  padding: 60px 20px;
  font-size: 1.1rem;
}

/* Responsive */
@media (max-width: 768px) {
  .modal-content {
    width: 95%;
    max-height: 85vh;
  }

  .modal-header h2 {
    font-size: 1.25rem;
  }

  .trailer-container {
    padding: 16px;
  }

  .trailer-container iframe {
    height: 300px !important;
  }
}
</style>
