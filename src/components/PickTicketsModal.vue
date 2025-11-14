<template>
  <!-- Pick Movies Modal -->
  <div v-if="showPickMoviesModal" class="modal-overlay" @click="closePickMoviesModal">
    <div class="modal-content pick-movies-modal" @click.stop>
      <div class="modal-header-pick">
        <h1>PICK {{ getStepLabel() }}</h1>
        <button class="close-btn" @click="closePickMoviesModal">Close</button>
      </div>

      <div class="pick-movies-container">
        <!-- Left Sidebar -->
        <div class="pick-sidebar">
          <div class="sidebar-section" v-if="selectedPickMovie">
            <label>Movies:</label>
            <p>{{ selectedPickMovie.title }}</p>
          </div>
          <div class="sidebar-section" v-if="selectedPickCinema">
            <label>Cinema:</label>
            <p>{{ selectedPickCinema.name }}</p>
          </div>
          <div class="sidebar-section" v-if="selectedPickShowType">
            <label>Show Type:</label>
            <p>{{ selectedPickShowType }}</p>
          </div>
          <div class="sidebar-section" v-if="selectedPickDate">
            <label>Date:</label>
            <p>{{ selectedPickDate }}</p>
          </div>
          <div class="sidebar-section" v-if="selectedPickTime">
            <label>Time:</label>
            <p>{{ selectedPickTime }}</p>
          </div>
          <div class="sidebar-buttons">
            <button class="sidebar-btn" @click="startAgain">Start Again</button>
            <button class="sidebar-btn" @click="closePickMoviesModal" v-if="selectedPickTime">Skip to Results</button>
          </div>
        </div>

        <!-- Main Content -->
        <div class="pick-content">
          <!-- PICK A TIME FLOW: Step 1 - Pick Time -->
          <div v-if="pickStep === 1 && modalPickType === 'TIME'" class="items-grid">
            <button 
              v-for="time in getAllShowTimes()"
              :key="time"
              class="item-btn"
              @click="selectPickTime(time)"
            >
              {{ time }}
            </button>
          </div>

          <!-- PICK A TIME FLOW: Step 2 - Pick Date -->
          <div v-else-if="pickStep === 2 && modalPickType === 'TIME'" class="items-grid">
            <button 
              v-for="date in getAvailableDates()"
              :key="date"
              class="item-btn"
              @click="selectPickDate(date)"
            >
              {{ date }}
            </button>
          </div>

          <!-- PICK A TIME FLOW: Step 3 - Pick Cinema -->
          <div v-else-if="pickStep === 3 && modalPickType === 'TIME'" class="items-grid">
            <button 
              v-for="cinema in cinemaData"
              :key="cinema.id"
              class="item-btn"
              @click="selectPickCinema(cinema)"
            >
              {{ cinema.name }}
            </button>
          </div>

          <!-- PICK A TIME FLOW: Step 4 - Pick Show Type -->
          <div v-else-if="pickStep === 4 && modalPickType === 'TIME'" class="items-grid">
            <button 
              v-for="type in getAllShowTypes()"
              :key="type"
              class="item-btn"
              @click="selectPickShowType(type)"
            >
              {{ type }}
            </button>
          </div>

          <!-- PICK A TIME FLOW: Step 5 - Pick Movie -->
          <div v-else-if="pickStep === 5 && modalPickType === 'TIME'" class="movies-grid">
            <div 
              v-for="movie in nowShowingMovies" 
              :key="movie.id"
              class="movie-card"
              @click="selectPickMovie(movie)"
            >
              <img :src="movie.poster" :alt="movie.title" />
              <p>{{ movie.title }}</p>
            </div>
          </div>

          <!-- PICK A MOVIE FLOW: Step 1 - Pick Movie -->
          <div v-else-if="pickStep === 1 && modalPickType === 'MOVIES'" class="movies-grid">
            <div 
              v-for="movie in nowShowingMovies" 
              :key="movie.id"
              class="movie-card"
              @click="selectPickMovie(movie)"
            >
              <img :src="movie.poster" :alt="movie.title" />
              <p>{{ movie.title }}</p>
            </div>
          </div>

          <!-- PICK A MOVIE FLOW: Step 2 - Pick Cinema -->
          <div v-else-if="pickStep === 2 && modalPickType === 'MOVIES'" class="items-grid">
            <button 
              v-for="cinema in cinemaData"
              :key="cinema.id"
              class="item-btn"
              @click="selectPickCinema(cinema)"
            >
              {{ cinema.name }}
            </button>
          </div>

          <!-- PICK A MOVIE FLOW: Step 3 - Pick Show Type -->
          <div v-else-if="pickStep === 3 && modalPickType === 'MOVIES'" class="items-grid">
            <button 
              v-for="type in getAllShowTypes()"
              :key="type"
              class="item-btn"
              @click="selectPickShowType(type)"
            >
              {{ type }}
            </button>
          </div>

          <!-- PICK A MOVIE FLOW: Step 4 - Pick Date -->
          <div v-else-if="pickStep === 4 && modalPickType === 'MOVIES'" class="items-grid">
            <button 
              v-for="date in getAvailableDates()"
              :key="date"
              class="item-btn"
              @click="selectPickDate(date)"
            >
              {{ date }}
            </button>
          </div>

          <!-- PICK A MOVIE FLOW: Step 5 - Pick Time -->
          <div v-else-if="pickStep === 5 && modalPickType === 'MOVIES'" class="items-grid">
            <button 
              v-for="time in getAllShowTimes()"
              :key="time"
              class="item-btn"
              @click="selectPickTime(time)"
            >
              {{ time }}
            </button>
          </div>

          <!-- PICK A CINEMA FLOW: Step 1 - Pick Cinema -->
          <div v-else-if="pickStep === 1 && modalPickType === 'CINEMA'" class="items-grid">
            <button 
              v-for="cinema in cinemaData"
              :key="cinema.id"
              class="item-btn"
              @click="selectPickCinema(cinema)"
            >
              {{ cinema.name }}
            </button>
          </div>

          <!-- PICK A CINEMA FLOW: Step 2 - Pick Show Type -->
          <div v-else-if="pickStep === 2 && modalPickType === 'CINEMA'" class="items-grid">
            <button 
              v-for="type in getAllShowTypes()"
              :key="type"
              class="item-btn"
              @click="selectPickShowType(type)"
            >
              {{ type }}
            </button>
          </div>

          <!-- PICK A CINEMA FLOW: Step 3 - Pick Movie -->
          <div v-else-if="pickStep === 3 && modalPickType === 'CINEMA'" class="movies-grid">
            <div 
              v-for="movie in nowShowingMovies" 
              :key="movie.id"
              class="movie-card"
              @click="selectPickMovie(movie)"
            >
              <img :src="movie.poster" :alt="movie.title" />
              <p>{{ movie.title }}</p>
            </div>
          </div>

          <!-- PICK A CINEMA FLOW: Step 4 - Pick Date -->
          <div v-else-if="pickStep === 4 && modalPickType === 'CINEMA'" class="items-grid">
            <button 
              v-for="date in getAvailableDates()"
              :key="date"
              class="item-btn"
              @click="selectPickDate(date)"
            >
              {{ date }}
            </button>
          </div>

          <!-- PICK A CINEMA FLOW: Step 5 - Pick Time -->
          <div v-else-if="pickStep === 5 && modalPickType === 'CINEMA'" class="items-grid">
            <button 
              v-for="time in getAllShowTimes()"
              :key="time"
              class="item-btn"
              @click="selectPickTime(time)"
            >
              {{ time }}
            </button>
          </div>

          <!-- PICK A SHOW TYPE FLOW: Step 1 - Pick Show Type -->
          <div v-else-if="pickStep === 1 && modalPickType === 'SHOW TYPE'" class="items-grid">
            <button 
              v-for="type in getAllShowTypes()"
              :key="type"
              class="item-btn"
              @click="selectPickShowType(type)"
            >
              {{ type }}
            </button>
          </div>

          <!-- PICK A SHOW TYPE FLOW: Step 2 - Pick Cinema (with that show type) -->
          <div v-else-if="pickStep === 2 && modalPickType === 'SHOW TYPE'" class="items-grid">
            <button 
              v-for="cinema in getCinemasWithShowType()"
              :key="cinema.id"
              class="item-btn"
              @click="selectPickCinema(cinema)"
            >
              {{ cinema.name }}
            </button>
          </div>

          <!-- PICK A SHOW TYPE FLOW: Step 3 - Pick Movie -->
          <div v-else-if="pickStep === 3 && modalPickType === 'SHOW TYPE'" class="movies-grid">
            <div 
              v-for="movie in nowShowingMovies" 
              :key="movie.id"
              class="movie-card"
              @click="selectPickMovie(movie)"
            >
              <img :src="movie.poster" :alt="movie.title" />
              <p>{{ movie.title }}</p>
            </div>
          </div>

          <!-- PICK A SHOW TYPE FLOW: Step 4 - Pick Date -->
          <div v-else-if="pickStep === 4 && modalPickType === 'SHOW TYPE'" class="items-grid">
            <button 
              v-for="date in getAvailableDates()"
              :key="date"
              class="item-btn"
              @click="selectPickDate(date)"
            >
              {{ date }}
            </button>
          </div>

          <!-- PICK A SHOW TYPE FLOW: Step 5 - Pick Time -->
          <div v-else-if="pickStep === 5 && modalPickType === 'SHOW TYPE'" class="items-grid">
            <button 
              v-for="time in getAllShowTimes()"
              :key="time"
              class="item-btn"
              @click="selectPickTime(time)"
            >
              {{ time }}
            </button>
          </div>

          <div class="search-box">
            <input 
              type="text" 
              placeholder="Search"
              v-model="pickSearchQuery"
              @keyup="filterPickResults"
            />
          </div>

          <!-- Navigation Buttons -->
          <div class="pick-navigation">
            <button v-if="pickStep > 1" class="nav-btn prev-btn" @click="pickStep--">← Previous</button>
            <button v-if="!isLastStep" class="nav-btn next-btn" @click="pickStep++">Next →</button>
            <button v-if="isLastStep" class="nav-btn submit-btn" @click="completeSelection">Complete Selection</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, inject } from 'vue'

const currentPage = inject('currentPage', { value: 'home' })
const bookingPageRef = inject('bookingPageRef', null)
const bookingData = inject('bookingData', null)
const showPickMoviesModal = ref(false)
const modalPickType = ref('')
const pickSearchQuery = ref('')
const selectedPickMovie = ref(null)
const selectedPickCinema = ref(null)
const selectedPickShowType = ref(null)
const selectedPickDate = ref(null)
const selectedPickTime = ref(null)
const pickStep = ref(1) // 1: Movie, 2: Cinema, 3: Show Type, 4: Date, 5: Time

// Sample now showing movies with showtimes
const nowShowingMovies = [
  {
    id: 1,
    title: 'Quezon',
    rating: 'PG',
    poster: 'https://images.justwatch.com/poster/307617/s718/godzilla-x-kong-the-new-empire.jpg',
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ',
    showtimes: ['05:45 PM', '08:30 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['05:45 PM', '08:30 PM'] },
      { date: 'Thu, Nov 15', showtimes: ['04:00 PM', '06:45 PM', '09:15 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['05:00 PM', '07:45 PM', '10:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['01:00 PM', '03:45 PM', '06:15 PM', '08:45 PM'] }
    ]
  },
  {
    id: 2,
    title: 'Meet, Greet & Bye',
    rating: 'G',
    poster: 'https://images.justwatch.com/poster/307617/s718/godzilla-x-kong-the-new-empire.jpg',
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ',
    showtimes: ['04:15 PM', '06:45 PM', '09:00 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['04:15 PM', '06:45 PM', '09:00 PM'] },
      { date: 'Thu, Nov 15', showtimes: ['02:30 PM', '05:00 PM', '07:30 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['03:00 PM', '05:30 PM', '08:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['11:00 AM', '01:30 PM', '04:00 PM', '06:30 PM'] }
    ]
  },
  {
    id: 3,
    title: 'The Running Man',
    rating: 'M',
    poster: 'https://images.justwatch.com/poster/307617/s718/godzilla-x-kong-the-new-empire.jpg',
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ',
    showtimes: ['05:00 PM', '07:30 PM', '10:00 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['05:00 PM', '07:30 PM', '10:00 PM'] },
      { date: 'Thu, Nov 15', showtimes: ['06:00 PM', '08:30 PM', '11:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['05:30 PM', '08:00 PM', '10:30 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['03:00 PM', '05:30 PM', '08:00 PM', '10:30 PM'] }
    ]
  }
]

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
      { id: 28, address: 'Governors Drive, Bgy Sampolok 1, Dasmarinas Cavite', types: ['Director\'s Club'] }
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

const openPickMoviesModal = (type) => {
  modalPickType.value = type
  pickSearchQuery.value = ''
  
  // Reset all selections
  selectedPickMovie.value = null
  selectedPickCinema.value = null
  selectedPickShowType.value = null
  selectedPickDate.value = null
  selectedPickTime.value = null
  
  // Set the starting step based on the type selected
  // Step 1 is always the first selection (the entry point)
  pickStep.value = 1
  
  showPickMoviesModal.value = true
}

const openPickMoviesModalWithMovie = (movie) => {
  // When user clicks "Buy Tickets" on a specific movie, pre-select that movie
  // and start at cinema selection (skip movie selection step)
  modalPickType.value = 'MOVIES'
  pickSearchQuery.value = ''
  
  // Pre-select the movie
  selectedPickMovie.value = movie
  
  // Reset other selections
  selectedPickCinema.value = null
  selectedPickShowType.value = null
  selectedPickDate.value = null
  selectedPickTime.value = null
  
  // Start at step 2 (Cinema selection) instead of step 1 (Movie selection)
  pickStep.value = 2
  
  showPickMoviesModal.value = true
}

const closePickMoviesModal = () => {
  showPickMoviesModal.value = false
  modalPickType.value = ''
}

const startAgain = () => {
  // Reset all selections
  selectedPickMovie.value = null
  selectedPickCinema.value = null
  selectedPickShowType.value = null
  selectedPickDate.value = null
  selectedPickTime.value = null
  pickStep.value = 1
}

const completeSelection = () => {
  // Store booking details in shared bookingData
  if (bookingData && bookingData.value) {
    bookingData.value.movie = selectedPickMovie.value
    bookingData.value.cinema = selectedPickCinema.value
    bookingData.value.date = selectedPickDate.value
    bookingData.value.time = selectedPickTime.value
    bookingData.value.showType = selectedPickShowType.value
  }
  
  showPickMoviesModal.value = false
  currentPage.value = 'booking'
}

const selectPickMovie = (movie) => {
  selectedPickMovie.value = movie
  if (pickStep.value === 5) {
    // Last step - complete selection
    completeSelection()
  } else {
    pickStep.value++ // Move to next step
  }
}

const selectPickCinema = (cinema) => {
  selectedPickCinema.value = cinema
  if (pickStep.value === 5) {
    // Last step - complete selection
    completeSelection()
  } else {
    pickStep.value++ // Move to next step
  }
}

const selectPickShowType = (type) => {
  selectedPickShowType.value = type
  if (pickStep.value === 5) {
    // Last step - complete selection
    completeSelection()
  } else {
    pickStep.value++ // Move to next step
  }
}

const selectPickDate = (date) => {
  selectedPickDate.value = date
  if (pickStep.value === 5) {
    // Last step - complete selection
    completeSelection()
  } else {
    pickStep.value++ // Move to next step
  }
}

const selectPickTime = (time) => {
  selectedPickTime.value = time
  if (pickStep.value === 5) {
    // Last step - complete selection
    completeSelection()
  } else {
    pickStep.value++ // Move to next step
  }
}

const getAllShowTypes = () => {
  // If a cinema is selected, return only types available at that cinema
  if (selectedPickCinema.value) {
    const types = new Set()
    selectedPickCinema.value.locations.forEach(location => {
      if (location.types) {
        location.types.forEach(type => types.add(type))
      } else if (location.type) {
        types.add(location.type)
      }
    })
    // Add default types if none found
    if (types.size === 0) {
      types.add('2D')
      types.add('3D')
    }
    return Array.from(types).sort()
  }
  
  // Otherwise return all available types
  const types = new Set()
  cinemaData.forEach(cinema => {
    cinema.locations.forEach(location => {
      if (location.types) {
        location.types.forEach(type => types.add(type))
      }
    })
  })
  types.add('2D')
  types.add('3D')
  return Array.from(types).sort()
}

const getAvailableDates = () => {
  // If a movie is selected, return dates for that movie
  if (selectedPickMovie.value) {
    return selectedPickMovie.value.dates || ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17']
  }
  // Otherwise return default dates
  return ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17']
}

const getAllShowTimes = () => {
  // If TIME is the first step, show all times
  if (modalPickType.value === 'TIME') {
    const times = new Set()
    nowShowingMovies.forEach(movie => {
      movie.showtimes.forEach(time => times.add(time))
      movie.futureShoTimes.forEach(futureDate => {
        futureDate.showtimes.forEach(time => times.add(time))
      })
    })
    return Array.from(times).sort()
  }
  
  // If a movie and date are selected, return times for that date
  if (selectedPickMovie.value && selectedPickDate.value) {
    const dateEntry = selectedPickMovie.value.futureShoTimes.find(d => d.date === selectedPickDate.value)
    if (dateEntry) {
      return dateEntry.showtimes.sort()
    }
  }
  
  // If only a movie is selected, return all times for that movie
  if (selectedPickMovie.value) {
    const times = new Set()
    selectedPickMovie.value.showtimes.forEach(time => times.add(time))
    selectedPickMovie.value.futureShoTimes.forEach(futureDate => {
      futureDate.showtimes.forEach(time => times.add(time))
    })
    return Array.from(times).sort()
  }
  
  // Otherwise return all available times
  const times = new Set()
  nowShowingMovies.forEach(movie => {
    movie.showtimes.forEach(time => times.add(time))
    movie.futureShoTimes.forEach(futureDate => {
      futureDate.showtimes.forEach(time => times.add(time))
    })
  })
  return Array.from(times).sort()
}

const filterPickResults = () => {
  // Search filtering would go here
}

const getStepLabel = () => {
  // Determine the label based on modalPickType and pickStep
  switch(modalPickType.value) {
    case 'TIME':
      if (pickStep.value === 1) return 'TIME'
      if (pickStep.value === 2) return 'DATE'
      if (pickStep.value === 3) return 'CINEMA'
      if (pickStep.value === 4) return 'SHOW TYPE'
      if (pickStep.value === 5) return 'MOVIE'
      break
    case 'MOVIES':
      if (pickStep.value === 1) return 'MOVIE'
      if (pickStep.value === 2) return 'CINEMA'
      if (pickStep.value === 3) return 'SHOW TYPE'
      if (pickStep.value === 4) return 'DATE'
      if (pickStep.value === 5) return 'TIME'
      break
    case 'CINEMA':
      if (pickStep.value === 1) return 'CINEMA'
      if (pickStep.value === 2) return 'SHOW TYPE'
      if (pickStep.value === 3) return 'MOVIE'
      if (pickStep.value === 4) return 'DATE'
      if (pickStep.value === 5) return 'TIME'
      break
    case 'SHOW TYPE':
      if (pickStep.value === 1) return 'SHOW TYPE'
      if (pickStep.value === 2) return 'CINEMA'
      if (pickStep.value === 3) return 'MOVIE'
      if (pickStep.value === 4) return 'DATE'
      if (pickStep.value === 5) return 'TIME'
      break
  }
  return 'SELECTION'
}

const getCinemasWithShowType = () => {
  // Return only cinemas that have the selected show type
  if (!selectedPickShowType.value) return cinemaData
  
  return cinemaData.filter(cinema => {
    return cinema.locations.some(location => {
      const types = location.types || (location.type ? [location.type] : [])
      return types.includes(selectedPickShowType.value)
    })
  })
}

const isLastStep = computed(() => {
  // All flows have 5 steps total
  return pickStep.value === 5
})

// Expose methods to parent component
defineExpose({
  openPickMoviesModal,
  openPickMoviesModalWithMovie,
  closePickMoviesModal,
  selectedPickMovie,
  selectedPickCinema,
  selectedPickShowType,
  selectedPickDate,
  selectedPickTime
})
</script>

<style scoped>
/* Modal Styles */
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
}

.modal-content {
  background-color: white;
  border-radius: 8px;
  width: 100%;
  max-width: 900px;
  max-height: 90vh;
  overflow-y: auto;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.3);
}

.pick-movies-modal {
  max-width: 1200px;
}

.modal-header-pick {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 2rem;
  background-color: rgb(249, 44, 29);
  color: white;
}

.modal-header-pick h1 {
  font-size: 2rem;
  margin: 0;
  font-weight: 700;
}

.close-btn {
  background-color: white;
  color: rgb(249, 44, 29);
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.close-btn:hover {
  background-color: #f0f0f0;
}

.pick-movies-container {
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 0;
  min-height: 500px;
}

.pick-sidebar {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.sidebar-section label {
  display: block;
  font-weight: 700;
  font-size: 0.9rem;
  margin-bottom: 0.5rem;
}

.sidebar-section p {
  margin: 0;
  font-size: 0.95rem;
  word-break: break-word;
}

.sidebar-buttons {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.sidebar-btn {
  background-color: white;
  color: rgb(249, 44, 29);
  border: none;
  padding: 0.75rem 1rem;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  font-size: 0.9rem;
  transition: all 0.3s ease;
  text-decoration: underline;
}

.sidebar-btn:hover {
  background-color: #f0f0f0;
}

.pick-content {
  padding: 2rem;
  display: flex;
  flex-direction: column;
}

.movies-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.movie-card {
  cursor: pointer;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  border-radius: 4px;
  overflow: hidden;
}

.movie-card:hover {
  transform: translateY(-4px);
}

.movie-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
  display: block;
}

.movie-card p {
  padding: 0.75rem;
  text-align: center;
  font-size: 0.9rem;
  font-weight: 600;
  background-color: #f9f9f9;
  margin: 0;
  min-height: 50px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.item-btn {
  padding: 1rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.9rem;
  text-align: center;
  line-height: 1.4;
}

.item-btn:hover {
  background-color: #D62828;
  transform: translateY(-2px);
}

.search-box {
  margin-top: auto;
}

.search-box input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s ease;
}

.search-box input:focus {
  outline: none;
  border-color: rgb(249, 44, 29);
}

.pick-navigation {
  display: flex;
  gap: 1rem;
  justify-content: flex-end;
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 1px solid #ddd;
}

.nav-btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  font-size: 1rem;
  transition: all 0.3s ease;
}

.prev-btn {
  background-color: white;
  border: 2px solid #ddd;
  color: #333;
}

.prev-btn:hover {
  border-color: rgb(249, 44, 29);
  color: rgb(249, 44, 29);
}

.next-btn {
  background-color: rgb(249, 44, 29);
  color: white;
}

.next-btn:hover {
  background-color: #D62828;
}

.submit-btn {
  background-color: rgb(249, 44, 29);
  color: white;
}

.submit-btn:hover {
  background-color: #D62828;
}

@media (max-width: 768px) {
  .pick-movies-container {
    grid-template-columns: 1fr;
  }

  .pick-sidebar {
    display: none;
  }

  .pick-content {
    padding: 1rem;
  }

  .movies-grid {
    grid-template-columns: repeat(auto-fill, minmax(120px, 1fr));
  }

  .items-grid {
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  }

  .modal-header-pick h1 {
    font-size: 1.5rem;
  }
}
</style>
