<template>
  <section class="events-section">
    <div class="hero-banner">
      <div class="banner-content">
        <div class="left-content">
          <h1>Book a Cinema</h1>
          <p>For booking, visit www.smcinema.com – Promos and details</p>
          <div class="qr-section">
            <div class="book-here">Book Here</div>
            <img src="https://via.placeholder.com/150x150?text=QR+Code" alt="QR Code" class="qr-code">
          </div>
        </div>

        <div class="right-content">
          <img src="https://via.placeholder.com/600x300?text=Cinema+Seats" alt="Cinema Seats" class="cinema-image">
        </div>
      </div>

      <div class="banner-footer">
        <p>By clicking the button below, you agree to the Terms and Conditions & Data Privacy Policy</p>
        <button @click="openBookingForm" class="book-now-btn">BOOK NOW</button>
      </div>

      <div class="brand-header">
        <span class="smcinema-text">SMCINEMA</span>
      </div>
    </div>
  </section>
</template>

<script setup>
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
  background: linear-gradient(135deg, #0052cc 0%, #003d99 100%);
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
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  align-items: center;
  margin-bottom: 1.5rem;
  flex: 1;
}

.left-content {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.left-content h1 {
  font-size: 2.5rem;
  font-weight: 700;
  color: white;
  margin: 0;
  line-height: 1.1;
}

.left-content p {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.9);
  margin: 0;
}

.qr-section {
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-top: 0.5rem;
}

.book-here {
  background-color: white;
  padding: 0.6rem 0.8rem;
  border-radius: 6px;
  font-weight: 700;
  color: #333;
  text-align: center;
  font-size: 0.75rem;
  line-height: 1.2;
  white-space: nowrap;
}

.qr-code {
  width: 110px;
  height: 110px;
  border-radius: 4px;
  background-color: white;
  padding: 0.3rem;
}

.right-content {
  display: flex;
  align-items: center;
  justify-content: center;
}

.cinema-image {
  width: 100%;
  height: auto;
  border-radius: 6px;
  object-fit: cover;
}

.banner-footer {
  text-align: center;
  padding: 1rem 0 0;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
}

.banner-footer p {
  color: white;
  font-size: 0.75rem;
  margin-bottom: 0.75rem;
  line-height: 1.4;
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
    grid-template-columns: 1fr;
    gap: 1rem;
    margin-bottom: 1rem;
  }

  .left-content h1 {
    font-size: 1.75rem;
  }

  .left-content p {
    font-size: 0.8rem;
  }

  .qr-section {
    flex-direction: column;
    align-items: center;
  }

  .qr-code {
    width: 80px;
    height: 80px;
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
