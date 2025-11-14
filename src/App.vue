<script setup>
import { ref, onMounted, watch, provide } from 'vue'
import Header from './components/Header.vue'
import MovieSection from './components/MovieSection.vue'
import CinemasPage from './components/CinemasPage.vue'
import ContactPage from './components/ContactPage.vue'
import EventsExperiencesPage from './components/EventsExperiencesPage.vue'
import LoyaltyPage from './components/LoyaltyPage.vue'
import ShopPage from './components/ShopPage.vue'
import RatingsPage from './components/RatingsPage.vue'
import BookingPage from './components/BookingPage.vue'
import Footer from './components/Footer.vue'

const currentPage = ref('home')
const bookingPageRef = ref(null)

// Shared booking data that flows from modal to booking page
const bookingData = ref({
  movie: null,
  cinema: null,
  date: null,
  time: null,
  showType: null
})

// Provide currentPage, bookingPageRef, and bookingData to child components
provide('currentPage', currentPage)
provide('bookingPageRef', bookingPageRef)
provide('bookingData', bookingData)

// Load current page from localStorage on mount
onMounted(() => {
  const savedPage = localStorage.getItem('currentPage')
  if (savedPage) {
    currentPage.value = savedPage
  }
})

// Save current page to localStorage whenever it changes
watch(currentPage, (newPage) => {
  localStorage.setItem('currentPage', newPage)
})
</script>

<template>
  <div id="app">
    <Header @navigate="currentPage = $event" />
    <MovieSection v-if="currentPage === 'home'" />
    <CinemasPage v-else-if="currentPage === 'cinemas'" @navigate="currentPage = $event" />
    <ContactPage v-else-if="currentPage === 'contact'" @navigate="currentPage = $event" />
    <EventsExperiencesPage v-else-if="currentPage === 'events'" @navigate="currentPage = $event" />
    <LoyaltyPage v-else-if="currentPage === 'loyalty'" @navigate="currentPage = $event" />
    <ShopPage v-else-if="currentPage === 'shop'" @navigate="currentPage = $event" />
    <RatingsPage v-else-if="currentPage === 'ratings'" @navigate="currentPage = $event" />
    <BookingPage v-else-if="currentPage === 'booking'" ref="bookingPageRef" @navigate="currentPage = $event" />
    <Footer />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: #f5f5f5;
}

#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
