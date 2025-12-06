<template>
  <section class="cinemas-section">
    <PickTicketsModal ref="pickTicketsModal" />

    <div class="cinemas-header">
      <div class="header-text">
        <p>SM Cinema locations across the Philippines</p>
        <h1>Find your nearest cinema</h1>
      </div>
      <div class="general-enquiries">
        <a href="mailto:info@smcinema.com">General enquiries</a>
      </div>
      <div class="search-wrapper">
        <input
          v-model="searchQuery"
          type="search"
          class="search-input"
          placeholder="Search cinemas or addresses"
        />
      </div>
    </div>

    <div class="filters">
      <button
        v-for="option in filterOptions"
        :key="option.key"
        class="filter-btn"
        :class="{ active: selectedFilter === option.key }"
        @click="selectedFilter = option.key"
      >
        {{ option.label }}
      </button>
    </div>

    <div class="cinema-grid">
      <article v-for="card in cinemaCards" :key="card.id" class="cinema-card">
        <div>
          <h2>{{ card.name }}</h2>
          <p class="cinema-address">{{ card.address }}</p>
        </div>
        <div class="card-meta">
          <span v-for="type in card.types" :key="type" class="type-badge">{{ type }}</span>
        </div>
        <button class="link-btn" @click="openMoviesModal(card)">See what's playing</button>
      </article>
    </div>

    <div v-if="showMoviesModal" class="modal-overlay" @click="closeMoviesModal">
      <div class="modal-content" @click.stop>
        <div class="modal-header">
          <button class="back-btn" @click="closeMoviesModal">← Back</button>
          <div>
            <h2>NOW SHOWING — {{ selectedLocation?.name }}</h2>
            <p v-if="selectedLocation" class="modal-address">{{ selectedLocation.address }}</p>
          </div>
        </div>
        <div class="modal-body">
          <div v-if="selectedLocation?.types?.length" class="modal-types">
            <span
              v-for="type in selectedLocation.types"
              :key="type"
              class="type-badge small"
            >
              {{ type }}
            </span>
          </div>

          <div class="showtimes-grid">
            <div v-for="movie in nowShowingMovies" :key="movie.id" class="movie-showtime">
              <img :src="movie.poster" :alt="movie.title" class="movie-poster-small" />
              <div class="movie-details">
                <h3>{{ movie.title }}</h3>
                <div class="rating-badge">{{ movie.rating }}</div>
                <div class="showtimes">
                  <span
                    v-for="showtime in movie.showtimes"
                    :key="showtime"
                    class="showtime-btn"
                  >
                    {{ showtime }}
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, ref } from 'vue'
import PickTicketsModal from './PickTicketsModal.vue'

const pickTicketsModal = ref(null)
const searchQuery = ref('')
const selectedFilter = ref('all')
const showMoviesModal = ref(false)
const selectedLocation = ref(null)

const filterOptions = [
  { key: 'all', label: 'All cinemas' },
  { key: 'directors', label: "Director's Club" },
  { key: 'imax', label: 'IMAX' }
]

const nowShowingMovies = [
  {
    id: 1,
    title: 'Meet, Greet & Bye',
    rating: 'G',
    poster: new URL('../assets/Now_Showing/MeetGreet-AndBye.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=7U9KZness2w',
    showtimes: ['03:30 PM', '06:00 PM', '08:30 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['03:30 PM', '06:00 PM', '08:30 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=71IG6R6VESA',
    showtimes: ['01:45 PM', '04:30 PM', '07:15 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['01:45 PM', '04:30 PM', '07:15 PM'] },
      { date: 'Thu, Nov 15', showtimes: ['02:30 PM', '05:00 PM', '08:00 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['03:15 PM', '06:00 PM', '09:00 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['12:30 PM', '03:15 PM', '06:00 PM', '08:45 PM'] }
    ]
  },
  {
    id: 3,
    title: "Now You See Me: Now You Don't",
    rating: 'PG-13',
    poster: new URL('../assets/Now_Showing/NowYouSeeMe-NowYouDont.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=Kna8sZ1pp4g',
    showtimes: ['02:00 PM', '05:00 PM', '07:45 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['02:00 PM', '05:00 PM', '07:45 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=2mWjBcA7zEY',
    showtimes: ['10:30 AM', '12:45 PM', '03:15 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['10:30 AM', '12:45 PM', '03:15 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=YxuY3hOs7QA',
    showtimes: ['06:00 PM', '08:45 PM', '11:30 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['06:00 PM', '08:45 PM', '11:30 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=aHrZseSM1n0',
    showtimes: ['02:30 PM', '05:30 PM', '08:30 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['02:30 PM', '05:30 PM', '08:30 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=lX6Qo0vZ9aw',
    showtimes: ['07:00 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['07:00 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=5D1v3xv_WoI',
    showtimes: ['04:00 PM', '07:00 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['04:00 PM', '07:00 PM'] },
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
    trailer: 'https://www.youtube.com/watch?v=9h8BSv4ICsA',
    showtimes: ['01:00 PM', '03:30 PM', '06:00 PM'],
    dates: ['Today', 'Thu, Nov 15', 'Fri, Nov 16', 'Sat, Nov 17'],
    futureShoTimes: [
      { date: 'Today', showtimes: ['01:00 PM', '03:30 PM', '06:00 PM'] },
      { date: 'Thu, Nov 15', showtimes: ['01:30 PM', '04:00 PM', '06:30 PM'] },
      { date: 'Fri, Nov 16', showtimes: ['02:00 PM', '05:00 PM', '07:30 PM'] },
      { date: 'Sat, Nov 17', showtimes: ['12:30 PM', '03:00 PM', '05:30 PM'] }
    ]
  }
]

const getLocationTypes = (location) =>
  location.types || (location.type ? [location.type] : [])

const filteredCinemas = computed(() => {
  let filtered = cinemaData

  if (selectedFilter.value !== 'all') {
    filtered = filtered
      .map((cinema) => ({
        ...cinema,
        locations: cinema.locations.filter((location) => {
          const types = getLocationTypes(location)
          if (selectedFilter.value === 'directors') {
            return types.some((type) => type && type.includes("Director"))
          }
          if (selectedFilter.value === 'imax') {
            return types.some((type) => type && type.includes('IMAX'))
          }
          return true
        })
      }))
      .filter((cinema) => cinema.locations.length > 0)
  }

  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    filtered = filtered
      .map((cinema) => ({
        ...cinema,
        locations: cinema.locations.filter((location) =>
          location.address.toLowerCase().includes(query) || cinema.name.toLowerCase().includes(query)
        )
      }))
      .filter((cinema) => cinema.locations.length > 0)
  }

  return filtered
})

const cinemaCards = computed(() =>
  filteredCinemas.value.flatMap((cinema) =>
    cinema.locations.map((location) => ({
      id: `${cinema.id}-${location.id}`,
      name: cinema.name,
      address: location.address,
      types: getLocationTypes(location),
      originalCinema: cinema
    }))
  )
)

const openMoviesModal = (card) => {
  selectedLocation.value = card
  showMoviesModal.value = true
}

const closeMoviesModal = () => {
  showMoviesModal.value = false
}

const cinemaData = [
  {
    id: 1,
    name: 'Light Residences',
    locations: [
      { id: 1, address: 'EDSA Cor Madison St, Brgy Barangka Ilaya, Mandaluyong City', type: null }
    ]
  },
  {
    id: 2,
    name: 'SM Aura Premier',
    locations: [
      {
        id: 3,
        address: '26th St Cor. Mc Kinley, Parkway Brgy Fort Bonifacio, Global City Taguig City 1630',
        types: ['Atmos', 'Director\'s Club', 'IMAX']
      }
    ]
  },
  {
    id: 3,
    name: 'SM CDO Downtown Premier',
    locations: [
      {
        id: 4,
        address: 'CM Recto Ave Cor Osmeña St, Bgry 24 Pob, Cagayan Misamis Oriental',
        types: ['Director\'s Club', 'Large Screen Format']
      }
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
      {
        id: 8,
        address: 'Plaridel-Pulilan Diversion Rd, Santo Cristo 3005, Pulilan Bulacan Philippines',
        type: null
      }
    ]
  },
  {
    id: 8,
    name: 'SM Center Sangandaan',
    locations: [
      {
        id: 9,
        address: 'Marcelo H Del Pilar St Cor Samson Road Brgy 003, Caloocan City',
        type: null
      }
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
      {
        id: 14,
        address: 'Lerma Street Ibayo 2100, City of Balanga (Capital), Bataan, Philippines',
        type: null
      }
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
      {
        id: 16,
        address: 'Dr. A. Santos Ave. Cor. Presidents Ave Brgy BF Homes, Paranaque City',
        types: ['Director\'s Club']
      }
    ]
  },
  {
    id: 16,
    name: 'SM City Bicutan',
    locations: [
      {
        id: 17,
        address: 'Dona Soledad Ave., Don Bosco, Fourth District, Paranaque City',
        types: ['Director\'s Club']
      }
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
      {
        id: 22,
        address: 'Deparo Road Zone 15, Barangay 171 District 1, Bagumbong 1421, Caloocan City',
        type: null
      }
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
      {
        id: 24,
        address: 'North Reclamation Area, Cebu City 6000',
        types: ['Director\'s Club', 'IMAX']
      }
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
      {
        id: 31,
        address: 'Quirino Hi-way cor Regalado Av, Bgy Greater Lagro, Novaliches Quezon City',
        types: ['Director\'s Club', 'Large Screen Format']
      }
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
      {
        id: 33,
        address: 'Rizal Avenue Extension, Barangay 88 Zone 8 District II, Grace Park East, Caloocan City',
        types: ['Director\'s Club']
      }
    ]
  },
  {
    id: 33,
    name: 'SM City Iloilo',
    locations: [
      {
        id: 34,
        address: 'Benigno Aquino Ave, Mandurriao Bolilao, Iloilo City',
        types: ['Director\'s Club', 'IMAX', 'Large Screen Format']
      }
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
      {
        id: 36,
        address: 'Along Diversion Road Biday, 2500 City of San Fernando, La Union Philippines',
        types: ['Director\'s Club']
      }
    ]
  },
  {
    id: 36,
    name: 'SM City Laoag',
    locations: [
      {
        id: 37,
        address: 'Airport Road Bgy. No. 51-B, Nangalisan West 2900, City of Laoag Ilocos Norte',
        types: ['Director\'s Club']
      }
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
      {
        id: 42,
        address: 'Marcos Highway, Kalumpang, Marikina City NCR Second',
        types: ['Director\'s Club']
      }
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
      {
        id: 45,
        address: 'La Purisima Street, Barangay Zone III (Pob), Zamboanga City Zamboanga Del Sur',
        type: null
      }
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
      {
        id: 47,
        address: 'Central Business District II Triangulo, Naga City, Camarines Sur',
        type: null
      }
    ]
  },
  {
    id: 47,
    name: 'SM City North Edsa',
    locations: [
      {
        id: 48,
        address: 'North EDSA PAG ASA I, Quezon City',
        types: ['Director\'s Club', 'IMAX']
      }
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
      {
        id: 52,
        address: 'San Jose, City of San Fernando Pampanga',
        types: ['Director\'s Club']
      }
    ]
  },
  {
    id: 52,
    name: 'SM City Puerto Princesa',
    locations: [
      {
        id: 53,
        address: 'Malvar Corner Lacao Streets, San Miguel, Puerto Princesa City Palawan',
        types: ['Director\'s Club']
      }
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
      {
        id: 56,
        address: 'Arnaldo Boulevard Roxas City, 5800 Roxas City (Capital), Capiz Philippines',
        type: null
      }
    ]
  },
  {
    id: 56,
    name: 'SM City San Jose del Monte',
    locations: [
      {
        id: 57,
        address: 'Quirino Highway, Brgy Tungkong Manga, San Jose Del Monte City Bulacan',
        type: null
      }
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
      {
        id: 62,
        address: 'Maharlika Highway, Balogo East District, City of Sorsogon, Sorsogon',
        type: null
      }
    ]
  },
  {
    id: 62,
    name: 'SM City Sta. Mesa',
    locations: [
      {
        id: 63,
        address: 'R. Magsaysay Cor G Araneta Ave, Dona Imelda, Quezon City',
        types: ['Director\'s Club']
      }
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
      {
        id: 65,
        address: 'Dr. A. Santos Ave., Brgy. San Dionisio, Paranaque City',
        type: null
      }
    ]
  },
  {
    id: 65,
    name: 'SM City Tanza',
    locations: [
      {
        id: 66,
        address: 'Antero Soriano Highway, Daang Amaya II 4108, Tanza Cavite Philippines',
        type: null
      }
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
      {
        id: 69,
        address: 'Governors Drive Cor Capitol Rd, Brgy San Agustin, Trece Martires City Cavite',
        type: null
      }
    ]
  },
  {
    id: 69,
    name: 'SM City Tuguegarao',
    locations: [
      {
        id: 70,
        address: 'Bagay Road (Tuguegarao-Solana), Caritan Norte, Tuguegarao City, Cagayan',
        type: null
      }
    ]
  },
  {
    id: 70,
    name: 'SM City Urdaneta Central',
    locations: [
      {
        id: 71,
        address: 'Mac Arthur Highway, Nancayasan 2428, City of Urdaneta Pangasinan',
        type: null
      }
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
      {
        id: 73,
        address: 'Mac Arthur Highway, Telabastagan, City of San Fernando Pampanga',
        type: null
      }
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
      {
        id: 75,
        address: 'JW Diokno Blvd CBP-IA, Pasay City',
        types: ['2D', 'Director\'s Club', 'Event Cinema', 'IMAX', 'S Maison', 'ScreenX']
      }
    ]
  },
  {
    id: 75,
    name: 'S Maison',
    locations: [
      {
        id: 76,
        address: 'Seaside Boulevard Corner Coral Way, Mall of Asia Complex, Pasay City',
        types: ['2D', 'Directors Club', 'S Maison']
      }
    ]
  },
  {
    id: 76,
    name: 'SM Megacenter Cabanatuan',
    locations: [
      {
        id: 77,
        address: 'Gen Tinio & Melencio Sts., Brgy. San Roque Norte, Cabanatuan City Nueva Ecija',
        type: null
      }
    ]
  },
  {
    id: 77,
    name: 'SM Megamall',
    locations: [
      {
        id: 78,
        address: 'J. Vargas Cor. EDSA, Wack-Wack Village, Mandaluyong City',
        types: ['Director\'s Club', 'IMAX']
      }
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
      {
        id: 80,
        address: 'South Road Properties 6000, Cebu City',
        types: ['Director\'s Club', 'Large Screen Format']
      }
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
      {
        id: 82,
        address: '12 ADB Avenue Ortigas Center, Brgy Wack-Wack Greenhills East, Mandaluyong City',
        types: ['Director\'s Club']
      }
    ]
  }
]
</script>

<style scoped>
.cinemas-section {
  padding: 3rem 1.25rem 4rem;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

.cinemas-header {
  max-width: 1200px;
  margin: 0 auto 2rem;
  display: grid;
  gap: 1rem;
}

.header-text h1 {
  margin: 0;
  font-size: 2.5rem;
  font-weight: 700;
  color: #222;
}

.header-text p {
  margin: 0;
  color: #555;
}

.general-enquiries {
  justify-self: end;
}

.general-enquiries a {
  color: #0066cc;
  text-decoration: none;
  font-weight: 600;
}

.search-wrapper {
  width: 100%;
}

.search-input {
  width: 100%;
  padding: 0.9rem 1.25rem;
  border-radius: 999px;
  border: 1px solid #ddd;
  font-size: 1rem;
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  background: white;
}

.search-input:focus {
  outline: none;
  border-color: #e63946;
  box-shadow: 0 0 0 4px rgba(230, 57, 70, 0.15);
}

.filters {
  max-width: 1200px;
  margin: 0 auto 2rem;
  display: flex;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.filter-btn {
  padding: 0.6rem 1.25rem;
  border-radius: 999px;
  border: 1px solid #ddd;
  background-color: white;
  font-size: 0.9rem;
  font-weight: 600;
  color: #444;
  cursor: pointer;
  transition: all 0.3s ease;
}

.filter-btn.active {
  background-color: #e63946;
  border-color: #e63946;
  color: white;
}

.filter-btn:hover {
  border-color: #e63946;
  color: #e63946;
}

.cinema-grid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.25rem;
}

.cinema-card {
  background: white;
  border-radius: 16px;
  padding: 1.5rem;
  border: 1px solid #e0e0e0;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.cinema-card h2 {
  font-size: 1.25rem;
  margin: 0;
  color: #222;
}

.cinema-address {
  margin: 0.35rem 0 0;
  color: #6b6b6b;
  font-size: 0.95rem;
}

.card-meta {
  display: flex;
  flex-wrap: wrap;
  gap: 0.35rem;
}

.type-badge {
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  padding: 0.25rem 0.6rem;
  border-radius: 999px;
  border: 1px solid #e63946;
  color: #e63946;
}

.type-badge.small {
  border-color: rgba(38, 0, 15, 0.2);
  color: #333;
}

.link-btn {
  margin-top: auto;
  align-self: flex-start;
  color: #e63946;
  background: none;
  border: none;
  padding: 0;
  font-weight: 600;
  cursor: pointer;
  transition: color 0.3s ease;
}

.link-btn:hover {
  color: #b1272d;
}

.modal-overlay {
  position: fixed;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 2rem;
}

.modal-content {
  background: white;
  border-radius: 12px;
  max-width: 1100px;
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
  border-bottom: 2px solid #e63946;
  background-color: #f9f9f9;
}

.back-btn {
  background-color: #e63946;
  color: white;
  border: none;
  padding: 0.75rem 1.5rem;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
}

.back-btn:hover {
  background-color: #d11721;
}

.modal-address {
  margin: 0.25rem 0 0;
  color: #555;
}

.modal-body {
  padding: 2rem;
}

.modal-types {
  margin-bottom: 1rem;
}

.showtimes-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 1.25rem;
}

.movie-showtime {
  border: 1px solid #ddd;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
}

.movie-poster-small {
  width: 100%;
  height: 180px;
  object-fit: cover;
}

.movie-details {
  padding: 1.25rem;
}

.movie-details h3 {
  font-size: 1.1rem;
  margin: 0 0 0.5rem;
  color: #333;
}

.rating-badge {
  display: inline-block;
  background-color: #e63946;
  color: white;
  padding: 0.25rem 0.75rem;
  border-radius: 4px;
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 1rem;
}

.showtimes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.showtime-btn {
  padding: 0.4rem 0.9rem;
  border-radius: 999px;
  border: 1px solid #e63946;
  color: #e63946;
  font-weight: 600;
  font-size: 0.85rem;
}

@media (max-width: 768px) {
  .cinemas-header {
    text-align: center;
  }

  .general-enquiries {
    justify-self: center;
  }

  .modal-header {
    flex-direction: column;
    align-items: flex-start;
  }
}
</style>
