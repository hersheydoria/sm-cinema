<template>
  <section class="ratings-section">
    <!-- Pick Tickets Modal Component -->
    <PickTicketsModal ref="pickTicketsModal" />

    <div class="container">
      <!-- Hero Banner -->
      <div class="ratings-banner">
        <div class="banner-content">
          <div class="banner-left">
            <h1>PICK TICKETS</h1>
            <p>Browse and Book sessions easily. Start with your choice of Movie, Cinema, Show Type or Time.</p>
            <div class="banner-buttons">
              <button class="banner-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('MOVIES')">PICK A<br>MOVIE</button>
              <button class="banner-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('CINEMA')">PICK A<br>CINEMA</button>
              <button class="banner-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('SHOW TYPE')">PICK A<br>SHOW TYPE</button>
              <button class="banner-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('TIME')">PICK A<br>TIME</button>
            </div>
          </div>
          <div class="banner-right">
            <div class="book-section">
              <div class="sm-logo">SM</div>
              <h2>SMCINEMA</h2>
              <h3>Book a Cinema</h3>
              <p>For booking, visit www.smcinema.com - Promos and Events</p>
              <div class="qr-code">
                <img src="https://via.placeholder.com/120x120?text=QR+Code" alt="Book Here QR Code">
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Ratings Definition -->
      <div class="ratings-content">
        <h2 class="section-title">DEFINITION OF THE RATINGS</h2>

        <!-- G Rating -->
        <div class="rating-item">
          <div class="rating-badge g-rating">G</div>
          <div class="rating-details">
            <h3>Suitable For General Audiences Of All Ages</h3>
            <p>A G-rated motion picture contains nothing in theme, language, nudity, sex, violence or other matters that, in the view of the Rating Board, would offend parents whose younger children view the motion picture. The G rating is not a "certificate of approval", nor does it signify a "children's" motion picture. Some snippets of language may go beyond polite conversation but they are common everyday expressions. No stronger words are present in G-rated motion pictures. Depictions of violence are minimal. No nudity, sex scenes or drug use are present in the motion picture.</p>
          </div>
        </div>

        <!-- PG Rating -->
        <div class="rating-item">
          <div class="rating-badge pg-rating">PG</div>
          <div class="rating-details">
            <h3>Parental Guidance Suggested. Some Material May Not Be Suitable For Children</h3>
            <p>A PG-rated motion picture should be investigated by parents before they let their younger children attend. The PG rating indicates, in the view of the Rating Board, that parents may consider some material unsuitable for their children, and parents should make that decision. The more mature themes in some PG-rated motion pictures may call for parental guidance. There may be some profanity and some depictions of violence or brief nudity. But these elements are not deemed so intense as to require that parents be strongly cautioned beyond the suggestion of parental guidance. There is no drug use content in a PG-rated motion picture.</p>
          </div>
        </div>

        <!-- R-13 Rating -->
        <div class="rating-item">
          <div class="rating-badge r13-rating">R-13</div>
          <div class="rating-details">
            <h3>Recommended For A Mature Audience</h3>
            <p>An MTRCB R-13 rating means the film is restricted to viewers 13 years and older. It indicates that the film may contain themes, language, violence, nudity, sex, horror, or drugs that may not be suitable for children under 13.</p>
          </div>
        </div>

        <!-- R-16 Rating -->
        <div class="rating-item">
          <div class="rating-badge r16-rating">R-16</div>
          <div class="rating-details">
            <h3>The Film Is Restricted To Viewers 16 Years Old And Above.</h3>
            <p>Only those 16 years of age and older are permitted to view the film. The rating signifies that the film may contain elements that are not appropriate for younger audience.</p>
          </div>
        </div>

        <!-- TFC Rating -->
        <div class="rating-item">
          <div class="rating-badge tfc-rating">TFC</div>
          <div class="rating-details">
            <h3>To Be Classified</h3>
            <p>This Film has not yet been classified.</p>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { inject } from 'vue'
import PickTicketsModal from './PickTicketsModal.vue'

const currentPage = inject('currentPage', { value: 'home' })
</script>

<style scoped>
.ratings-section {
  padding: 3rem 0;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Hero Banner */
.ratings-banner {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  background-color: rgb(249, 44, 29);
  padding: 2rem;
  border-radius: 8px;
  margin-bottom: 3rem;
}

.banner-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  align-items: center;
}

.banner-left {
  color: white;
}

.banner-left h1 {
  font-size: 1.8rem;
  font-weight: 700;
  color: white;
  margin-bottom: 0.5rem;
}

.banner-left p {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 1.5rem;
  line-height: 1.6;
  font-size: 0.95rem;
}

.banner-buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.banner-btn {
  padding: 1rem;
  background-color: transparent;
  color: white;
  border: 2px solid white;
  border-radius: 4px;
  font-weight: 700;
  font-size: 0.9rem;
  cursor: pointer;
  transition: all 0.3s ease;
  line-height: 1.4;
}

.banner-btn:hover {
  background-color: white;
  color: rgb(249, 44, 29);
}

.banner-right {
  background: linear-gradient(135deg, #0052cc 0%, #003d99 100%);
  padding: 2rem;
  border-radius: 4px;
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
}

.book-section {
  text-align: center;
}

.sm-logo {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: white;
}

.book-section h2 {
  font-size: 1.2rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: white;
}

.book-section h3 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 0.5rem;
  color: white;
}

.book-section p {
  font-size: 0.85rem;
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 1rem;
  line-height: 1.4;
}

.qr-code {
  display: flex;
  justify-content: center;
}

.qr-code img {
  width: 120px;
  height: 120px;
  border: 3px solid white;
  border-radius: 4px;
}

/* Ratings Content */
.ratings-content {
  background-color: white;
  padding: 3rem;
  border-radius: 8px;
}

.section-title {
  font-size: 1.8rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 2rem;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.rating-item {
  display: grid;
  grid-template-columns: 100px 1fr;
  gap: 2rem;
  margin-bottom: 2.5rem;
  padding-bottom: 2.5rem;
  border-bottom: 1px solid #eee;
  align-items: flex-start;
}

.rating-item:last-child {
  border-bottom: none;
  margin-bottom: 0;
  padding-bottom: 0;
}

.rating-badge {
  width: 80px;
  height: 80px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 1.2rem;
  color: white;
  flex-shrink: 0;
}

.g-rating {
  background-color: #28a745;
}

.pg-rating {
  background-color: #0052cc;
}

.r13-rating {
  background-color: #ffc107;
  color: #333;
}

.r16-rating {
  background-color: #ff6b35;
}

.tfc-rating {
  background-color: #6c757d;
}

.rating-details h3 {
  font-size: 1.2rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.75rem;
}

.rating-details p {
  color: #555;
  line-height: 1.6;
  font-size: 0.95rem;
}

/* Responsive Design */
@media (max-width: 768px) {
  .banner-content {
    grid-template-columns: 1fr;
  }

  .banner-buttons {
    grid-template-columns: 1fr 1fr;
  }

  .rating-item {
    grid-template-columns: 1fr;
    gap: 1rem;
  }

  .rating-badge {
    width: 60px;
    height: 60px;
    font-size: 1rem;
  }

  .ratings-content {
    padding: 1.5rem;
  }

  .section-title {
    font-size: 1.3rem;
  }
}
</style>
