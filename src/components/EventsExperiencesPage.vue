<template>
  <section class="events-section">
    <div class="hero-banner" :style="heroBannerStyle">
      <div class="banner-content"></div>

      <div class="banner-footer">
        <button @click="openBookingForm" class="book-now-btn">BOOK NOW</button>
      </div>
    </div>
  </section>
</template>

<script setup>
const eventBanner = new URL('../assets/Banners/EventsAndExperince_Banner/Banner_01.png', import.meta.url).href

const heroBannerStyle = {
  backgroundImage: `url(${eventBanner})`,
  backgroundSize: 'cover',
  backgroundPosition: 'center',
  backgroundRepeat: 'no-repeat',
}

const openBookingForm = () => {
  window.open('https://forms.office.com/pages/responsepage.aspx?id=DQSIkWdsW0yxEjajBLZtrQAAAAAAAAAAAAMAAEWc3ElUMTVBMjRPR0pWUVVLRktBMElCNUJLWjlHQi4u&route=shorturl', '_blank')
}
</script>

<style scoped>
.events-section {
  padding: 2rem;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
  display: flex;
  align-items: center;
  justify-content: center;
}

.hero-banner {
  background-color: transparent;
  padding: 2rem;
  width: 100%;
  max-width: 1050px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  position: relative;
  overflow: hidden;
  border-radius: 8px;
  aspect-ratio: 16 / 9;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

.brand-header {
  position: absolute;
  top: 1.5rem;
  right: 1.5rem;
  color: white;
  font-weight: 700;
  font-size: 0.95rem;
  letter-spacing: 0.5px;
}

.banner-content {
  flex: 1;
  min-height: 320px;
  margin-bottom: 1.5rem;
}

.banner-footer {
  text-align: center;
  padding: 0.5rem 0;
  margin-bottom: 160px;
}

.book-now-btn {
  padding: 0.5rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.3s ease;
  display: inline-block;
}

.book-now-btn:hover {
  background-color: #D62828;
  transform: scale(1.05);
}

@media (max-width: 768px) {
  .events-section {
    padding: 1rem;
  }

  .hero-banner {
    padding: 1.5rem;
    aspect-ratio: auto;
    min-height: auto;
  }

  .brand-header {
    position: static;
    margin-bottom: 1rem;
    text-align: center;
  }

  .banner-content {
    min-height: 260px;
    margin-bottom: 1rem;
  }

  .banner-footer {
    padding: 1rem 0 0;
  }

  .banner-footer p {
    font-size: 0.7rem;
  }

  .book-now-btn {
    padding: 0.4rem 1rem;
    font-size: 0.75rem;
  }
}
</style>
