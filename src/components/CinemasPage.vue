<template>
  <section class="cinemas-section">
    <!-- Pick Movies Modal Component -->
    <PickTicketsModal ref="pickTicketsModal" />

    <!-- Movies Modal -->
    <div v-if="showMoviesModal" class="modal-overlay" @click="closeMoviesModal">
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <button class="back-btn" @click="closeMoviesModal">← Back</button>
          <h2>NOW SHOWING - {{ selectedCinema?.name }}</h2>
        </div>
        
        <!-- Cinema Info Section -->
        <div class="cinema-info-section">
          <div class="cinema-info-box">
            <h3>Parking</h3>
            <p>Available</p>
          </div>
          <div class="cinema-info-box">
            <h3>Public Transport</h3>
            <p>Accessible</p>
          </div>
        </div>

        <!-- Theater Types Section -->
        <div class="theater-types-section">
          <div class="types-grid">
            <div v-for="type in getTheaterTypes()" :key="type" class="type-item">
              <span class="type-name">{{ type }}</span>
            </div>
          </div>
        </div>

        <div class="showtimes-grid">
          <div 
            v-for="movie in nowShowingMovies" 
            :key="movie.id" 
            class="movie-showtime"
          >
            <img :src="movie.poster" :alt="movie.title" class="movie-poster-small" />
            <div class="movie-details">
              <h3>{{ movie.title }}</h3>
              <div class="rating-badge">{{ movie.rating }}</div>
              
              <!-- Date Selection Buttons -->
              <div class="date-buttons">
                <button 
                  class="date-btn"
                  :class="{ active: getSelectedDateIndex(movie.id) === 0 }"
                  @click="selectDate(movie.id, 0)"
                >
                  Today
                </button>
                <button 
                  v-for="(futureDate, index) in movie.futureShoTimes" 
                  :key="futureDate.date"
                  class="date-btn"
                  :class="{ active: getSelectedDateIndex(movie.id) === index + 1 }"
                  @click="selectDate(movie.id, index + 1)"
                >
                  {{ futureDate.date }}
                </button>
              </div>

              <!-- Showtimes for Selected Date -->
              <div class="showtimes">
                <button 
                  v-for="time in getMovieShowtimesForDate(movie, getSelectedDateIndex(movie.id))" 
                  :key="time" 
                  class="showtime-btn"
                >
                  {{ time }}
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button class="print-btn" @click="printShowtimes">Print Preview</button>
        </div>
      </div>
    </div>

    <!-- Pick Tickets Modal Component -->
    <PickTicketsModal ref="pickTicketsModal" />

    <!-- Pick Tickets Banner -->
    <div class="banner">
      <div class="banner-content">
        <div class="banner-left">
          <h2>PICK TICKETS</h2>
          <p>Browse and Book sessions easily. Start with your choice of Movie, Cinema, Show Type or Time.</p>
          <div class="banner-buttons">
            <button class="banner-btn" @click="openPickMoviesModal('MOVIES')">PICK A<br>MOVIE</button>
            <button class="banner-btn" @click="openPickMoviesModal('CINEMA')">PICK A<br>CINEMA</button>
            <button class="banner-btn" @click="openPickMoviesModal('SHOW TYPE')">PICK A<br>SHOW TYPE</button>
            <button class="banner-btn" @click="openPickMoviesModal('TIME')">PICK A<br>TIME</button>
          </div>
        </div>
        <div
          class="banner-right"
          @mouseenter="pauseBannerRotation"
          @mouseleave="resumeBannerRotation"
        >
          <img :src="currentBanner" alt="Cinema banner" />
          <div class="banner-text">
            <div class="imax-logo"></div>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div class="general-enquiries">
        <a href="#" @click.prevent="navigateTo('contact')">General Enquiries</a>
      </div>

      <div class="cinemas-header">
        <div class="search-container">
          <input 
            type="text" 
            placeholder="Search cinemas..." 
            class="search-input"
            v-model="searchQuery"
          />
        </div>
      </div>

      <div class="filters">
        <button 
          class="filter-btn"
          :class="{ active: selectedFilter === 'all' }"
          @click="selectedFilter = 'all'"
        >
          ALL CINEMA LOCATIONS
        </button>
        <button 
          class="filter-btn"
          :class="{ active: selectedFilter === 'directors' }"
          @click="selectedFilter = 'directors'"
        >
          Director's Club
        </button>
        <button 
          class="filter-btn"
          :class="{ active: selectedFilter === 'imax' }"
          @click="selectedFilter = 'imax'"
        >
          IMAX
        </button>
      </div>

      <div class="cinemas-list">
        <div 
          v-for="cinema in filteredCinemas" 
          :key="cinema.id"
          class="cinema-group"
        >
          <h2>{{ cinema.name }}</h2>
          <div class="locations">
            <div 
              v-for="location in cinema.locations" 
              :key="location.id"
              class="location-card"
            >
              <h3>{{ location.address }}</h3>
              <div v-if="location.types || location.type" class="location-types">
                <span v-for="t in (location.types || [location.type])" :key="t" class="type-badge">{{ t }}</span>
              </div>
            </div>
          </div>
          <div class="see-whats-playing">
            <button class="link-btn" @click="openMoviesModal(cinema)">See What's Playing</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, inject, onMounted, onBeforeUnmount } from 'vue'
import PickTicketsModal from './PickTicketsModal.vue'

const currentPage = inject('currentPage', { value: 'home' })
const searchQuery = ref('')
const selectedFilter = ref('all')
const showMoviesModal = ref(false)
const selectedCinema = ref(null)
const showFutureDates = ref(false)
const selectedDateIndexByMovie = ref({})
const pickTicketsModal = ref(null)
const cinemaBanners = [
  new URL('../assets/Banners/Cinema_Banners/banner_1.jpeg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_2.jpg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_3.jpg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_4.jpeg', import.meta.url).href
]
const currentBannerIndex = ref(0)
const currentBanner = computed(() => cinemaBanners[currentBannerIndex.value])
let bannerInterval = null

const rotateBanner = () => {
  currentBannerIndex.value = (currentBannerIndex.value + 1) % cinemaBanners.length
}

const pauseBannerRotation = () => {
  if (bannerInterval) {
    clearInterval(bannerInterval)
    bannerInterval = null
  }
}

const resumeBannerRotation = () => {
  if (!bannerInterval) {
    bannerInterval = setInterval(rotateBanner, 5000)
  }
}

onMounted(() => {
  resumeBannerRotation()
})

onBeforeUnmount(() => {
  pauseBannerRotation()
})

const navigateTo = (page) => {
  currentPage.value = page
}

// Generate future dates (next 3 days)
const generateFutureDates = () => {
  const dates = []
  const today = new Date()
  for (let i = 1; i <= 3; i++) {
    const date = new Date(today)
    date.setDate(date.getDate() + i)
    dates.push({
      date: date,
      dateString: date.toLocaleDateString('en-US', { weekday: 'short', month: 'short', day: 'numeric' })
    })
  }
  return dates
}

// Sample now showing movies with showtimes
const nowShowingMovies = [
  {
    id: 1,
    title: 'Meet, Greet & Bye',
    rating: 'G',
    poster: new URL('../assets/Now_Showing/MeetGreet-AndBye.jpg', import.meta.url).href,
    showtimes: ['03:30 PM', '06:00 PM', '08:30 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['04:00 PM', '06:45 PM', '09:15 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['05:00 PM', '07:45 PM', '10:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['01:00 PM', '03:45 PM', '06:15 PM', '08:45 PM'] }
    ]
  },
  {
    id: 2,
    title: 'Wicked: For Good',
    rating: 'PG-13',
    poster: new URL('../assets/Now_Showing/The-Wicked.jpg', import.meta.url).href,
    showtimes: ['01:45 PM', '04:30 PM', '07:15 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['02:30 PM', '05:00 PM', '08:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['03:15 PM', '06:00 PM', '09:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['12:30 PM', '03:15 PM', '06:00 PM', '08:45 PM'] }
    ]
  },
  {
    id: 3,
    title: 'Now You See Me: Now You Don\'t',
    rating: 'PG-13',
    poster: new URL('../assets/Now_Showing/NowYouSeeMe-NowYouDont.jpg', import.meta.url).href,
    showtimes: ['02:00 PM', '05:00 PM', '07:45 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['03:00 PM', '06:00 PM', '09:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['03:30 PM', '07:00 PM', '10:15 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['01:30 PM', '04:30 PM', '07:15 PM', '09:45 PM'] }
    ]
  },
  {
    id: 4,
    title: 'Zootopia 2',
    rating: 'G',
    poster: new URL('../assets/Now_Showing/Zootopia-2.jpg', import.meta.url).href,
    showtimes: ['10:30 AM', '12:45 PM', '03:15 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['11:00 AM', '01:30 PM', '04:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['11:45 AM', '02:15 PM', '04:45 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['09:30 AM', '12:00 PM', '02:30 PM', '05:00 PM'] }
    ]
  },
  {
    id: 5,
    title: 'Tha Rae: The Exorcist',
    rating: 'M',
    poster: new URL('../assets/Now_Showing/TheRae-TheExorcist.jpg', import.meta.url).href,
    showtimes: ['06:00 PM', '08:45 PM', '11:30 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['06:30 PM', '09:15 PM', '11:45 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['07:00 PM', '09:30 PM', '12:00 AM'] },
      { date: 'Sat, Nov 17', showtimes: ['05:45 PM', '08:30 PM', '11:15 PM'] }
    ]
  },
  {
    id: 6,
    title: 'Salvageland',
    rating: 'PG-13',
    poster: new URL('../assets/Now_Showing/Salvage-Land.jpg', import.meta.url).href,
    showtimes: ['02:30 PM', '05:30 PM', '08:30 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['03:15 PM', '06:15 PM', '09:15 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['03:45 PM', '07:30 PM', '10:30 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['02:00 PM', '05:00 PM', '08:00 PM'] }
    ]
  },
  {
    id: 7,
    title: 'SEVENTEEN WORLD TOUR [NEW_] IN JAPAN: LIVE VIEWING',
    rating: 'G',
    poster: new URL('../assets/Now_Showing/SEVENTEEN_WORLD_TOUR_NEW_IN_JAPAN_LIVE_VIEWING.jpg', import.meta.url).href,
    showtimes: ['07:00 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['07:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['07:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['07:00 PM'] }
    ]
  },
  {
    id: 8,
    title: "KMJS' Gabi Ng Lagim: The Movie",
    rating: 'PG-13',
    poster: new URL("../assets/Now_Showing/KMJS'GabiNgLagim-TheMovie.jpg", import.meta.url).href,
    showtimes: ['04:00 PM', '07:00 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['04:45 PM', '07:30 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['05:30 PM', '08:15 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['03:00 PM', '06:00 PM'] }
    ]
  },
  {
    id: 9,
    title: 'Keeper',
    rating: 'PG-13',
    poster: new URL('../assets/Now_Showing/Keeper.jpg', import.meta.url).href,
    showtimes: ['01:00 PM', '03:30 PM', '06:00 PM'],
    futureShoTimes: [
      { date: 'Thu, Nov 15', showtimes: ['01:30 PM', '04:00 PM', '06:30 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['02:00 PM', '05:00 PM', '07:30 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['12:30 PM', '03:00 PM', '05:30 PM'] }
    ]
  }
]

const openMoviesModal = (cinema) => {
  selectedCinema.value = cinema
  showMoviesModal.value = true
}

const closeMoviesModal = () => {
  showMoviesModal.value = false
  selectedCinema.value = null
}

const printShowtimes = () => {
  const printWindow = window.open('', '_blank')
  const cinemaName = selectedCinema.value?.name || 'Cinema'
  const moviesHTML = nowShowingMovies.map(movie => `
    <div style="margin-bottom: 2rem; page-break-inside: avoid;">
      <h3 style="margin: 0 0 0.5rem 0; font-size: 1.1rem;">${movie.title}</h3>
      <div style="margin-bottom: 1rem;">
        <span style="display: inline-block; background-color: #E63946; color: white; padding: 0.25rem 0.75rem; border-radius: 4px; font-size: 0.85rem; font-weight: 600;">${movie.rating}</span>
      </div>
      <div style="display: flex; flex-wrap: wrap; gap: 0.5rem;">
        ${movie.showtimes.map(time => `<span style="padding: 0.5rem 1rem; border: 1px solid #E63946; border-radius: 4px; font-weight: 600;">${time}</span>`).join('')}
      </div>
    </div>
  `).join('')

  printWindow.document.write(`
    <!DOCTYPE html>
    <html>
    <head>
      <title>Now Showing - ${cinemaName}</title>
      <style>
        body { font-family: Arial, sans-serif; margin: 2rem; }
        h1 { color: #E63946; border-bottom: 2px solid #E63946; padding-bottom: 1rem; margin-bottom: 2rem; }
        h3 { color: #333; }
      </style>
    </head>
    <body>
      <h1>NOW SHOWING - ${cinemaName}</h1>
      ${moviesHTML}
    </body>
    </html>
  `)
  printWindow.document.close()
  printWindow.print()
}

const toggleFutureDates = () => {
  showFutureDates.value = !showFutureDates.value
}

const selectDate = (movieId, index) => {
  selectedDateIndexByMovie.value[movieId] = index
}

const getMovieShowtimesForDate = (movie, dateIndex) => {
  if (dateIndex === 0) {
    return movie.showtimes
  }
  return movie.futureShoTimes[dateIndex - 1]?.showtimes || []
}

const getSelectedDateIndex = (movieId) => {
  return selectedDateIndexByMovie.value[movieId] !== undefined ? selectedDateIndexByMovie.value[movieId] : 0
}

const openPickMoviesModal = (type) => {
  if (pickTicketsModal.value) {
    pickTicketsModal.value.openPickMoviesModal(type)
  }
}

const getTheaterTypes = () => {
  if (!selectedCinema.value || !selectedCinema.value.locations) return []
  const types = new Set()
  selectedCinema.value.locations.forEach(location => {
    if (location.types) {
      location.types.forEach(type => types.add(type))
    } else if (location.type) {
      types.add(location.type)
    }
  })
  return Array.from(types)
}

const cinemaData = [
  {
    id: 1,
    name: 'Light Residences',
    locations: [
      { id: 1, address: 'EDSA Cor Madison St, Brgy Barangka Ilaya, Mandaluyong City', type: null },
    ]
  },
  {
    id: 2,
    name: 'SM Aura Premier',
    locations: [
      { id: 3, address: '26th St Cor. Mc Kinley, Parkway Brgy Fort Bonifacio, Global City Taguig City 1630', types: ['Atmos', 'Director\'s Club', 'IMAX'] }
    ]
  },
  {
    id: 3,
    name: 'SM CDO Downtown Premier',
    locations: [
      { id: 4, address: 'CM Recto Ave Cor Osmeña St, Bgry 24 Pob, Cagayan Misamis Oriental', types: ['Director\'s Club', 'Large Screen Format'] }
    ]
  },
  {
    id: 4,
    name: 'SM Center Angono',
    locations: [
      { id: 5, address: 'Manila East Road, Brgy San Isidro, 1930 Angono Rizal', type: null }
    ]
  },
  {
    id: 5,
    name: 'SM Center Muntinlupa',
    locations: [
      { id: 6, address: 'National RD, Brgy Tunasan, Muntinlupa City', type: null }
    ]
  },
  {
    id: 6,
    name: 'SM Center Ormoc',
    locations: [
      { id: 7, address: 'Real Street Barangay 14 (Pob.), Ormoc City', type: null }
    ]
  },
  {
    id: 7,
    name: 'SM Center Pulilan',
    locations: [
      { id: 8, address: 'Plaridel-Pulilan Diversion Rd, Santo Cristo 3005, Pulilan Bulacan Philippines', type: null }
    ]
  },
  {
    id: 8,
    name: 'SM Center Sangandaan',
    locations: [
      { id: 9, address: 'Marcelo H Del Pilar St Cor Samson Road Brgy 003, Caloocan City', type: null }
    ]
  },
  {
    id: 9,
    name: 'SM City Bacolod',
    locations: [
      { id: 10, address: 'Reclamation Area, Bacolod City Philippines 6100', types: ['2D'] }
    ]
  },
  {
    id: 10,
    name: 'SM City Bacoor',
    locations: [
      { id: 11, address: 'Aguinaldo Hi-way, BO Habay II, Bacoor, Cavite', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 11,
    name: 'SM City Baguio',
    locations: [
      { id: 12, address: 'Luneta Hill, Upper Session Road, Baguio City', type: null }
    ]
  },
  {
    id: 12,
    name: 'SM City Baliwag',
    locations: [
      { id: 13, address: 'Dona Remedios Trinidad Highway Brgy Pagala, Baliwag Bulacan', type: null }
    ]
  },
  {
    id: 13,
    name: 'SM City Bataan',
    locations: [
      { id: 14, address: 'Lerma Street Ibayo 2100, City of Balanga (Capital), Bataan, Philippines', type: null }
    ]
  },
  {
    id: 14,
    name: 'SM City Batangas',
    locations: [
      { id: 15, address: 'Pastor Village, Brgy. Pallocan Kanluran, Batangas City', type: null }
    ]
  },
  {
    id: 15,
    name: 'SM City BF Paranaque',
    locations: [
      { id: 16, address: 'Dr. A. Santos Ave. Cor. Presidents Ave Brgy BF Homes, Paranaque City', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 16,
    name: 'SM City Bicutan',
    locations: [
      { id: 17, address: 'Dona Soledad Ave., Don Bosco, Fourth District, Paranaque City', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 17,
    name: 'SM City Butuan',
    locations: [
      { id: 18, address: 'JC Aquino Avenue, Barangay Lapu-lapu, Butuan City', type: null }
    ]
  },
  {
    id: 18,
    name: 'SM City Cabanatuan',
    locations: [
      { id: 19, address: 'Along Maharlika Highway, Brgy H Concepcion, Cabanatuan City', type: null }
    ]
  },
  {
    id: 19,
    name: 'SM City Cagayan de Oro',
    locations: [
      { id: 20, address: 'Brgy Upper Carmen, Cagayan De Oro City, Misamis Oriental', type: null }
    ]
  },
  {
    id: 20,
    name: 'SM City Calamba',
    locations: [
      { id: 21, address: 'National Road, Brgy Real, Calamba City Laguna', type: null }
    ]
  },
  {
    id: 21,
    name: 'SM City Caloocan',
    locations: [
      { id: 22, address: 'Deparo Road Zone 15, Barangay 171 District 1, Bagumbong 1421, Caloocan City', type: null }
    ]
  },
  {
    id: 22,
    name: 'SM City Cauayan',
    locations: [
      { id: 23, address: 'Maharlika Highway, Brgy. District II, Cauayan City, Isabela', type: null }
    ]
  },
  {
    id: 23,
    name: 'SM City Cebu',
    locations: [
      { id: 24, address: 'North Reclamation Area, Cebu City 6000', types: ['Director\'s Club', 'IMAX'] }
    ]
  },
  {
    id: 24,
    name: 'SM City Clark',
    locations: [
      { id: 25, address: 'M.A. Roxas Highway, Malabanias Angeles City', types: ['IMAX'] }
    ]
  },
  {
    id: 25,
    name: 'SM City Consolacion',
    locations: [
      { id: 26, address: 'Brgy. Lamac Consolacion, Cebu', type: null }
    ]
  },
  {
    id: 26,
    name: 'SM City Daet',
    locations: [
      { id: 27, address: 'Vinzons Avenue, Barangay Lag-on, Daet, Camarines Norte', type: null }
    ]
  },
  {
    id: 27,
    name: 'SM City Dasmarinas',
    locations: [
      { id: 28, address: 'Governors Drive, Bgy Sampalok 1, Dasmarinas Cavite', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 28,
    name: 'SM City Davao',
    locations: [
      { id: 29, address: 'Quimpo Blvd. Ecoland Subd., Brgy. Matina, Davao City', type: null }
    ]
  },
  {
    id: 29,
    name: 'SM City East Ortigas',
    locations: [
      { id: 30, address: 'Avenue Extension, Brgy Sta. Lucia, Pasig City', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 30,
    name: 'SM City Fairview',
    locations: [
      { id: 31, address: 'Quirino Hi-way cor Regalado Av, Bgy Greater Lagro, Novaliches Quezon City', types: ['Director\'s Club', 'Large Screen Format'] }
    ]
  },
  {
    id: 31,
    name: 'SM City General Santos',
    locations: [
      { id: 32, address: 'Cor. Santiago Blvd., San Miguel St Brgy Lagao, Gen Santos City', type: null }
    ]
  },
  {
    id: 32,
    name: 'SM City Grand Central',
    locations: [
      { id: 33, address: 'Rizal Avenue Extension, Barangay 88 Zone 8 District II, Grace Park East, Caloocan City', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 33,
    name: 'SM City Iloilo',
    locations: [
      { id: 34, address: 'Benigno Aquino Ave, Mandurriao Bolilao, Iloilo City', types: ['Director\'s Club', 'IMAX', 'Large Screen Format'] }
    ]
  },
  {
    id: 34,
    name: 'SM City J Mall Cebu',
    locations: [
      { id: 35, address: 'A.S. Fortuna St., Bakilid 6014, Mandaue City Cebu', type: null }
    ]
  },
  {
    id: 35,
    name: 'SM City La Union',
    locations: [
      { id: 36, address: 'Along Diversion Road Biday, 2500 City of San Fernando, La Union Philippines', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 36,
    name: 'SM City Laoag',
    locations: [
      { id: 37, address: 'Airport Road Bgy. No. 51-B, Nangalisan West 2900, City of Laoag Ilocos Norte', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 37,
    name: 'SM City Legazpi',
    locations: [
      { id: 38, address: 'Imelda Roces Avenue, Zone 9 Bgy 37 Bitano, Legazpi City Albay', type: null }
    ]
  },
  {
    id: 38,
    name: 'SM City Lipa',
    locations: [
      { id: 39, address: 'JP Laurel Highway, Brgy Sabang, Lipa City Batangas', type: null }
    ]
  },
  {
    id: 39,
    name: 'SM City Lucena',
    locations: [
      { id: 40, address: 'Dalahican Rd cor Maharlika, Brgy Ibabang Dupay, Lucena City', type: null }
    ]
  },
  {
    id: 40,
    name: 'SM City Manila',
    locations: [
      { id: 41, address: 'Concepcion Cor Arroceros And San Marcelino St, Ermita, Manila City', type: null }
    ]
  },
  {
    id: 41,
    name: 'SM City Marikina',
    locations: [
      { id: 42, address: 'Marcos Highway, Kalumpang, Marikina City NCR Second', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 42,
    name: 'SM City Marilao',
    locations: [
      { id: 43, address: 'Mc Arthur Highway, Brgy Ibayo, Marilao, Bulacan', type: null }
    ]
  },
  {
    id: 43,
    name: 'SM City Masinag',
    locations: [
      { id: 44, address: 'Marcos Highway, Mayamot, Antipolo City Rizal', type: null }
    ]
  },
  {
    id: 44,
    name: 'SM City Mindpro',
    locations: [
      { id: 45, address: 'La Purisima Street, Barangay Zone III (Pob), Zamboanga City Zamboanga Del Sur', type: null }
    ]
  },
  {
    id: 45,
    name: 'SM City Molino',
    locations: [
      { id: 46, address: 'Bgy Molino IV, Bacoor Cavite', type: null }
    ]
  },
  {
    id: 46,
    name: 'SM City Naga',
    locations: [
      { id: 47, address: 'Central Business District II Triangulo, Naga City, Camarines Sur', type: null }
    ]
  },
  {
    id: 47,
    name: 'SM City North Edsa',
    locations: [
      { id: 48, address: 'North EDSA PAG ASA I, Quezon City', types: ['Director\'s Club', 'IMAX'] }
    ]
  },
  {
    id: 48,
    name: 'SM City Novaliches',
    locations: [
      { id: 49, address: 'Quirino Highway, Bgy San Bartolome, Novaliches Quezon City', type: null }
    ]
  },
  {
    id: 49,
    name: 'SM City Olongapo Central',
    locations: [
      { id: 50, address: 'Rizal Avenue, Brgy East Tapinac, Olongapo City Zambales', types: ['2D'] }
    ]
  },
  {
    id: 50,
    name: 'SM City Olongapo Downtown',
    locations: [
      { id: 51, address: 'Magsaysay Drive Cor Gordon Ave, Pag-asa, Olongapo City', type: null }
    ]
  },
  {
    id: 51,
    name: 'SM City Pampanga',
    locations: [
      { id: 52, address: 'San Jose, City of San Fernando Pampanga', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 52,
    name: 'SM City Puerto Princesa',
    locations: [
      { id: 53, address: 'Malvar Corner Lacao Streets, San Miguel, Puerto Princesa City Palawan', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 53,
    name: 'SM City Rosales',
    locations: [
      { id: 54, address: 'Carmen East, Rosales Pangasinan', type: null }
    ]
  },
  {
    id: 54,
    name: 'SM City Rosario',
    locations: [
      { id: 55, address: 'General Trias Drive, Brgy Tejero, Rosario Cavite', type: null }
    ]
  },
  {
    id: 55,
    name: 'SM City Roxas',
    locations: [
      { id: 56, address: 'Arnaldo Boulevard Roxas City, 5800 Roxas City (Capital), Capiz Philippines', type: null }
    ]
  },
  {
    id: 56,
    name: 'SM City San Jose del Monte',
    locations: [
      { id: 57, address: 'Quirino Highway, Brgy Tungkong Manga, San Jose Del Monte City Bulacan', type: null }
    ]
  },
  {
    id: 57,
    name: 'SM City San Lazaro',
    locations: [
      { id: 58, address: 'Felix Huertas Cor AH Lacson Sts., Sta. Cruz Manila', type: null }
    ]
  },
  {
    id: 58,
    name: 'SM City San Mateo',
    locations: [
      { id: 59, address: 'Gen A Luna Avenue, Brgy Ampid I, San Mateo Rizal', type: null }
    ]
  },
  {
    id: 59,
    name: 'SM City San Pablo',
    locations: [
      { id: 60, address: 'National Highway, Brgy San Rafael, San Pablo City, Laguna', type: null }
    ]
  },
  {
    id: 60,
    name: 'SM City Santa Rosa',
    locations: [
      { id: 61, address: 'Nat\'l. Hi-Way Tagapo, Santa Rosa Laguna', type: null }
    ]
  },
  {
    id: 61,
    name: 'SM City Sorsogon',
    locations: [
      { id: 62, address: 'Maharlika Highway, Balogo East District, City of Sorsogon, Sorsogon', type: null }
    ]
  },
  {
    id: 62,
    name: 'SM City Sta. Mesa',
    locations: [
      { id: 63, address: 'R. Magsaysay Cor G Araneta Ave, Dona Imelda, Quezon City', types: ['Director\'s Club'] }
    ]
  },
  {
    id: 63,
    name: 'SM City Sto Tomas',
    locations: [
      { id: 64, address: 'Maharlika Highway, San Bartolome 4234, Santo Tomas Batangas', type: null }
    ]
  },
  {
    id: 64,
    name: 'SM City Sucat',
    locations: [
      { id: 65, address: 'Dr. A. Santos Ave., Brgy. San Dionisio, Paranaque City', type: null }
    ]
  },
  {
    id: 65,
    name: 'SM City Tanza',
    locations: [
      { id: 66, address: 'Antero Soriano Highway, Daang Amaya II 4108, Tanza Cavite Philippines', type: null }
    ]
  },
  {
    id: 66,
    name: 'SM City Tarlac',
    locations: [
      { id: 67, address: 'Mc Arthur Highway, San Roque Tarlac City', type: null }
    ]
  },
  {
    id: 67,
    name: 'SM City Taytay',
    locations: [
      { id: 68, address: 'Manila East Road, Taytay, Rizal', type: null }
    ]
  },
  {
    id: 68,
    name: 'SM City Trece Martires',
    locations: [
      { id: 69, address: 'Governors Drive Cor Capitol Rd, Brgy San Agustin, Trece Martires City Cavite', type: null }
    ]
  },
  {
    id: 69,
    name: 'SM City Tuguegarao',
    locations: [
      { id: 70, address: 'Bagay Road (Tuguegarao-Solana), Caritan Norte, Tuguegarao City, Cagayan', type: null }
    ]
  },
  {
    id: 70,
    name: 'SM City Urdaneta Central',
    locations: [
      { id: 71, address: 'Mac Arthur Highway, Nancayasan 2428, City of Urdaneta Pangasinan', type: null }
    ]
  },
  {
    id: 71,
    name: 'SM City Valenzuela',
    locations: [
      { id: 72, address: 'McArthur Highway, Brgy Karuhatan Valenzuela City', type: null }
    ]
  },
  {
    id: 72,
    name: 'SM City Telabastagan',
    locations: [
      { id: 73, address: 'Mac Arthur Highway, Telabastagan, City of San Fernando Pampanga', type: null }
    ]
  },
  {
    id: 73,
    name: 'SM Lanang Premier',
    locations: [
      { id: 74, address: 'J. P. Laurel Ave., Brgy San Antonio Agdao, Davao City, Davao Del Sur', types: ['IMAX'] }
    ]
  },
  {
    id: 74,
    name: 'SM Mall of Asia',
    locations: [
      { id: 75, address: 'JW Diokno Blvd CBP-IA, Pasay City', types: ['2D', 'Director\'s Club', 'Event Cinema', 'IMAX', 'S Maison', 'ScreenX'] }
    ]
  },
  {
    id: 75,
    name: 'S Maison',
    locations: [
      { id: 76, address: 'Seaside Boulevard Corner Coral Way, Mall of Asia Complex, Pasay City', types: ['2D', 'Directors Club', 'S Maison'] }
    ]
  },
  {
    id: 76,
    name: 'SM Megacenter Cabanatuan',
    locations: [
      { id: 77, address: 'Gen Tinio & Melencio Sts., Brgy. San Roque Norte, Cabanatuan City Nueva Ecija', type: null }
    ]
  },
  {
    id: 77,
    name: 'SM Megamall',
    locations: [
      { id: 78, address: 'J. Vargas Cor. EDSA, Wack-Wack Village, Mandaluyong City', types: ['Director\'s Club', 'IMAX'] }
    ]
  },
  {
    id: 78,
    name: 'SM San Fernando',
    locations: [
      { id: 79, address: 'Downtown, V. Tiomico St., Brgy. Sto. Rosario, San Fernando City, Pampanga', type: null }
    ]
  },
  {
    id: 79,
    name: 'SM Seaside City Cebu',
    locations: [
      { id: 80, address: 'South Road Properties 6000, Cebu City', types: ['Director\'s Club', 'Large Screen Format'] }
    ]
  },
  {
    id: 80,
    name: 'SM Southmall',
    locations: [
      { id: 81, address: 'Alabang Zapote Road, Almanza, Las Piñas City', types: ['Atmos', 'Director\'s Club'] }
    ]
  },
  {
    id: 81,
    name: 'The Podium Mall',
    locations: [
      { id: 82, address: '12 ADB Avenue Ortigas Center, Brgy Wack-Wack Greenhills East, Mandaluyong City', types: ['Director\'s Club'] }
    ]
  }
]

const filteredCinemas = computed(() => {
  let filtered = cinemaData

  if (selectedFilter.value !== 'all') {
    filtered = filtered.map(cinema => ({
      ...cinema,
      locations: cinema.locations.filter(location => {
        const types = location.types || (location.type ? [location.type] : [])
        if (selectedFilter.value === 'directors') {
          return types.some(t => t && t.includes('Director\'s Club'))
        }
        if (selectedFilter.value === 'imax') {
          return types.some(t => t && t.includes('IMAX'))
        }
        return true
      })
    })).filter(cinema => cinema.locations.length > 0)
  }

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered.map(cinema => ({
      ...cinema,
      locations: cinema.locations.filter(location => 
        location.address.toLowerCase().includes(query) ||
        cinema.name.toLowerCase().includes(query)
      )
    })).filter(cinema => cinema.locations.length > 0)
  }

  return filtered
})
</script>

<style scoped>
.cinemas-section {
  padding: 0;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

/* Pick Tickets Section */
.banner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  background-color: rgb(249, 44, 29);
  padding: 2rem;
  border-radius: 8px;
  margin-bottom: 3rem;
  margin-top: 3rem;
  align-items: center;
  max-width: 1135px;
  margin-left: auto;
  margin-right: auto;
}

.banner-content {
  display: contents;
}

.banner-left {
  color: white;
}

.banner-left h2 {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  margin-bottom: 0.5rem;
}

.banner-left p {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 1.5rem;
  line-height: 1.6;
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
  position: relative;
  overflow: hidden;
  flex: 1;
  padding: 0;
  border-radius: 0;
  background: transparent;
}

.banner-right img {
  width: 120%;
  height: auto;
  object-fit: cover;
  display: block;
}

.banner-text {
  position: absolute;
  inset: 0;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  gap: 0.4rem;
  background: linear-gradient(180deg, rgba(0, 0, 0, 0) 40%, rgba(0, 0, 0, 0.75));
  z-index: 1;
  color: white;
}

.imax-logo {
  color: #00a8ff;
  font-weight: 700;
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
}

.banner-title {
  color: white;
  font-size: 1.1rem;
  font-weight: 700;
  line-height: 1.3;
}

.banner-subtitle {
  color: white;
  font-size: 1.1rem;
  font-weight: 700;
  line-height: 1.3;
}

.general-enquiries {
  text-align: right;
  padding: 1rem 2rem;

}

.general-enquiries a {
  color: #0066cc;
  text-decoration: underline;
  font-weight: 600;
  transition: color 0.3s ease;
}

.general-enquiries a:hover {
  color: #E63946;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}



.search-container {
  margin-bottom: 1rem;
}

.search-input {
  width: 100%;
  max-width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
  background-color: white;
}

.search-input:focus {
  outline: none;
  border-color: #E63946;
}

.filters {
  display: flex;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 0.75rem 1.5rem;
  background-color: white;
  border: 2px solid #ddd;
  border-radius: 4px;
  color: #333;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-btn:hover {
  border-color: #E63946;
  color: #E63946;
}

.filter-btn.active {
  background-color: rgb(249, 44, 29);
  color: white;
  background-color: rgb(249, 44, 29);
}

.cinemas-list {
  margin-bottom: 2rem;
}

.cinema-group {
  margin-bottom: 2rem;
}

.cinema-group h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: #333;
  border-top: 2px solid #ddd;
  padding-top: 1rem;
}

.locations {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.location-card {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.location-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.location-card h3 {
  font-size: 1rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  color: #333;
}

.location-type {
  font-size: 0.85rem;
  color: #E63946;
  font-weight: 600;
  margin: 0.5rem 0 0 0;
}

.see-whats-playing {
  text-align: left;
}

.link-btn {
  color: #0066cc;
  background: none;
  border: none;
  font-weight: 600;
  cursor: pointer;
  padding: 0;
  font-size: 1rem;
  transition: color 0.3s ease;
  text-decoration: none;
}

.link-btn:hover {
  color: #E63946;
}

.link {
  color: #0066cc;
  text-decoration: none;
  font-weight: 600;
  transition: color 0.3s ease;
}

.link:hover {
  color: #E63946;
}


/* Modal Styles for See What's Playing */

.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 2rem;
  overflow-y: auto;
}

.modal-content {
  background-color: white;
  border-radius: 8px;
  max-width: 1200px;
  width: 100%;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.modal-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 2rem;
  border-bottom: 2px solid #E63946;
  background-color: #f9f9f9;
}

.back-btn {
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
  font-size: 1rem;
}

.back-btn:hover {
  background-color: #D62828;
}

.modal-header h2 {
  flex: 1;
  margin: 0;
  font-size: 1.5rem;
  color: #333;
}

.cinema-info-section {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  padding: 2rem;
  background-color: #f9f9f9;
  border-bottom: 1px solid #ddd;
}

.cinema-info-box {
  padding: 1.5rem;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.cinema-info-box h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1rem;
  color: #333;
  font-weight: 600;
}

.cinema-info-box p {
  margin: 0;
  color: #666;
  font-size: 0.95rem;
}

.theater-types-section {
  padding: 2rem;
  background-color: white;
  border-bottom: 2px solid #E63946;
}

.types-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1.5rem;
}

.type-item {
  text-align: center;
  padding: 1rem;
}

.type-name {
  display: block;
  font-weight: 600;
  color: #333;
  font-size: 0.95rem;
}

.showtimes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 2rem;
  padding: 2rem;
}

.movie-showtime {
  border: 1px solid #ddd;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.movie-poster-small {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.movie-details {
  padding: 1.5rem;
}

.movie-details h3 {
  font-size: 1.1rem;
  font-weight: 600;
  margin: 0 0 0.5rem 0;
  color: #333;
}

.rating-badge {
  display: inline-block;
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 1rem;
}

.date-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.date-btn {
  padding: 0.4rem 0.8rem;
  background-color: white;
  border: 2px solid #ddd;
  color: #333;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  font-size: 0.8rem;
  transition: all 0.3s ease;
}

.date-btn:hover {
  border-color: #E63946;
  color: #E63946;
}

.date-btn.active {
  background-color: rgb(249, 44, 29);
  border-color: rgb(249, 44, 29);
  color: white;
}

.showtimes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  align-items: center;
}

.date-label {
  display: block;
  width: 100%;
  font-weight: 700;
  color: #333;
  font-size: 0.9rem;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}

.future-dates {
  margin-top: 1rem;
  padding-top: 1rem;
  border-top: 1px solid #ddd;
}

.future-date-group {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
  align-items: center;
  margin-bottom: 0.75rem;
}

.future-date-group .date-label {
  margin: 0;
}

.showtime-btn {
  padding: 0.5rem 1rem;
  background-color: white;
  border: 2px solid #E63946;
  color: #E63946;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.9rem;
}

.showtime-btn:hover {
  background-color: rgb(249, 44, 29);
  color: white;
}

.modal-footer {
  display: flex;
  gap: 1rem;
  padding: 2rem;
  border-top: 1px solid #ddd;
  background-color: #f9f9f9;
  justify-content: flex-end;
}

.print-btn,
.dates-btn {
  padding: 0.75rem 1.5rem;
  background-color: white;
  border: 2px solid #ddd;
  color: #333;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 1rem;
}

.print-btn:hover,
.dates-btn:hover {
  border-color: #E63946;
  color: #E63946;
}

@media print {
  .modal-header,
  .modal-footer,
  .back-btn {
    display: none;
  }

  .modal-overlay {
    background-color: white;
  }

  .modal-content {
    box-shadow: none;
    max-width: 100%;
    max-height: 100%;
  }

  .showtimes-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
  }
}

@media (max-width: 768px) {
  .banner-content {
    flex-direction: column;
  }

  .banner-left h2 {
    font-size: 1.5rem;
  }

  .banner-buttons {
    grid-template-columns: repeat(2, 1fr);
  }

  .banner-btn {
    padding: 0.75rem 1rem;
    font-size: 0.8rem;
    background-color: white;
  }

  .banner-right {
    min-height: 220px;
    width: 100%;
  }

  .imax-logo {
    font-size: 1rem;
  }

  .banner-title {
    font-size: 0.9rem;
  }

  .banner-subtitle {
    font-size: 1.5rem;
  }

  .cinemas-header h1 {
    font-size: 1.5rem;
  }

  .filters {
    flex-direction: column;
  }

  .filter-btn {
    width: 100%;
  }

  .locations {
    grid-template-columns: 1fr;
  }
}
</style>
