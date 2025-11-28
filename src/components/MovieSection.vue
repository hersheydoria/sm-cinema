<template>
  <section class="movies-section">
    <!-- Pick Tickets Modal -->
    <PickTicketsModal ref="pickTicketsModal" />

    <!-- Trailer Modal -->
    <TrailerModal ref="trailerModal" />

    <div class="container">
      <div
        class="banner-carousel"
        @mouseenter="pauseBannerRotation"
        @mouseleave="resumeBannerRotation"
      >
        <button class="banner-control prev" type="button" aria-label="Previous banner" @click="prevBanner">
          ‹
        </button>
        <div class="banner-slide">
          <img :src="currentBanner" alt="Cinema banner" />
        </div>
        <button class="banner-control next" type="button" aria-label="Next banner" @click="nextBanner">
          ›
        </button>
      </div>

      <div class="section-header">
        <div class="tabs">
          <button
            v-for="tab in tabs"
            :key="tab"
            class="tab"
            :class="{ active: activeTab === tab }"
            @click="activeTab = tab"
          >
            {{ tab }}
          </button>
        </div>
      </div>

      <div class="movies-grid">
        <MovieCard
          v-for="movie in displayedMovies"
          :key="movie.id"
          :movie="movie"
          @buy-tickets="openPickTicketsModal($event)"
          @watch-trailer="openTrailer($event)"
        />
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'
import MovieCard from './MovieCard.vue'
import PickTicketsModal from './PickTicketsModal.vue'
import TrailerModal from './TrailerModal.vue'
const pickTicketsModal = ref(null)
const trailerModal = ref(null)
const activeTab = ref('NOW SHOWING')
const tabs = ['NOW SHOWING', 'COMING SOON']
const standardPrice = '₱350'
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

const prevBanner = () => {
  currentBannerIndex.value =
    (currentBannerIndex.value - 1 + cinemaBanners.length) % cinemaBanners.length
}

const nextBanner = () => {
  rotateBanner()
}

const openPickTicketsModal = (movie) => {
  if (pickTicketsModal.value) {
    pickTicketsModal.value.openPickMoviesModalWithMovie(movie)
  }
}

const openTrailer = (movie) => {
  if (trailerModal.value) {
    trailerModal.value.openTrailer(movie)
  }
}

const nowShowingMovies = [
  {
    id: 1,
    title: 'Meet, Greet & Bye',
    rating: 'G',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/MeetGreet-AndBye.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/Mtou4LuxFrg?si=8-vsDNPXYbHdaUHQ'
  },
  {
    id: 2,
    title: 'Wicked: For Good',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/The-Wicked.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/R2Xubj7lazE?si=7DfsSII3eHKA5cSf'
  },
  {
    id: 3,
    title: "Now You See Me: Now You Don't",
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/NowYouSeeMe-NowYouDont.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/-E3lMRx7HRQ?si=syCXRQHpMtLEi63W'
  },
  {
    id: 4,
    title: 'Zootopia 2',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Zootopia-2.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/BjkIOU5PhyQ?si=gwTO3__5o-OQExgA'
  },
  {
    id: 5,
    title: 'Tha Rae: The Exorcist',
    rating: 'M',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/TheRae-TheExorcist.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/TshZVPCrtF0?si=TYIbw9AX8PcRGc1L'
  },
  {
    id: 6,
    title: 'Salvageland',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Salvage-Land.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/Y1ClNzed-g4?si=AKL8BHQ3o_SJbuB5'
  },
  {
    id: 7,
    title: 'SEVENTEEN WORLD TOUR [NEW_] IN JAPAN: LIVE VIEWING',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/SEVENTEEN_WORLD_TOUR_NEW_IN_JAPAN_LIVE_VIEWING.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/EoGFOvMTBhM?si=k5LS-DyRLrwxilWj'
  },
  {
    id: 8,
    title: "KMJS' Gabi Ng Lagim: The Movie",
    rating: 'PG',
    price: standardPrice,
    poster: new URL("../assets/Now_Showing/KMJS'GabiNgLagim-TheMovie.jpg", import.meta.url).href,
    trailer: 'https://youtu.be/IeW72gqVPhE?si=NE8VXDjfabfFjZjh'
  },
  {
    id: 9,
    title: 'Keeper',
    rating: 'M',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Keeper.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/cwpusY785l4?si=QMgqIFKhL_Q7JQE4'
  }
]

const comingSoonMovies = [
  {
    id: 1,
    title: 'FFF2025 - 13 Days, 13 Nights',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-13Days13Nights.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/K-8nIAQit3o?si=0dkcp7ow5OPFKvL5'
  },
  {
    id: 2,
    title: 'FFF2025 - Diary of a Fleeting Affair',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-DiaryOfAFleetingAffair.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/K4Q95OIa-Pc?si=Ke6bOt4XlODHrgHE'
  },
  {
    id: 3,
    title: 'FFF2025 - Leave One Day',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-LeaveOneDay.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/qcyZMsj0TDA?si=VzCvAOBQbHAGtyZf'
  },
  {
    id: 4,
    title: 'FFF2025 - Maya, Give Me A Title',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-MayaGiveMeATitle.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/xI7XXNBilMM?si=aQko5gB2gLFB5b0H'
  },
  {
    id: 5,
    title: 'FFF2025 - On the Wandering Path',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-OnTheWanderingPath.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/nPLOSNIKEc0?si=lS9hGiihnOr76iqL'
  },
  {
    id: 6,
    title: 'FFF2025 - The King and The Mockingbird',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-TheKingAndTheMockingbird.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/4tE5902k3nE?si=Z5fx_C0L2d9SFKWQ'
  },
  {
    id: 7,
    title: 'FFF2025 - The Little Sister',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-TheLittleSister.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/aHntkAmfiNs?si=bEyh7uCmBFLrbo6C'
  },
  {
    id: 8,
    title: 'FFF2025 - The Richest Woman in the World',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-TheRichestWomanInTheWorld.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/UrqG3doFXUY?si=U7GeFKYlHR9wDwDn'
  },
  {
    id: 9,
    title: 'FFF2025 - The Shrinking Man',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FFF2025-TheShrinkingMan.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
  },
  {
    id: 10,
    title: 'Ang Happy Homes Ni Diane Hilario',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/AngHappyHomes-NiDianeHilario.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/QMUkKo16T7I?si=DAHM5wZeNGmdLcxt'
  },
  {
    id: 11,
    title: 'Eternity',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Eternity.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/irXTps1REHU?si=C7YjzimOIUXaqWpM'
  },
  {
    id: 12,
    title: "Five Nights at Freddy's 2",
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FiveNightsAtFreddys2.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/dSDpoobO6yM?si=G89UENqWN0mL2kD7'
  },
  {
    id: 13,
    title: 'Jackstone 5',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Jackstone-5.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/KmvqpgI-g1c?si=_5j6kzXNy5AERcvj'
  },
  {
    id: 14,
    title: 'JUJUTSU KAISEN: Shibuya Incident×The Culling Game',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/JUJUTSUKAISEN-ShibuyaIncident×TheCullingGame.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/-wkgtm0nsOg?si=THY_4QYHs_HbmPxe'
  },
  {
    id: 15,
    title: 'MONSTA X: CONNECT X IN CINEMAS',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/MONSTAX-CONNECTXINCINEMAS.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/eodDzdSNX8U?si=3h2Vvm5S_-djrzRU'
  },
  {
    id: 16,
    title: 'Nasaan si Hesus?',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Nasaansi-Hesus.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
  },
  {
    id: 17,
    title: "The Carpenter's Son",
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-CarpentersSon.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/aszdL5VFS6Q?si=2wZD6hdtuM-7yUrM'
  },
  {
    id: 18,
    title: 'The Ghost Village',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-GhostVillage.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
  },
  {
    id: 19,
    title: 'Light of the World',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/LightOf-TheWorld.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/rGd9zO_lvU4?si=NrIv0hLaygWlbSp8'
  },
  {
    id: 20,
    title: 'Scarlet',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Scarlet.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/XNar7yF-pf4?si=vQc1Pf0zOXykMg2_'
  },
  {
    id: 21,
    title: 'The Incredible Shrinking Man',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-IncredibleShrinkingMan.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/dX1E-9jE4bs?si=tht11onjNHTdCRwo'
  },
  {
    id: 22,
    title: 'The Shining 45th Year Anniversary',
    rating: 'R',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-Shining45thYearAnniversary.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/RmQPBzJKxcw?si=CeWP_eHv4yfz8o6l'
  },
  {
    id: 23,
    title: 'Top Gun: Maverick - Screen X',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/TopGun-Maverick-ScreenX.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
  },
  {
    id: 24,
    title: 'Avatar: Fire and Ash',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Avatar-FireandAsh.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/zO9GTJotxEQ?si=VjB8Tc7Scc8JEm4z'
  },
  {
    id: 25,
    title: 'Bar Boys: After School',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/BarBoys-AfterSchool.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/K4fQCnApsW8?si=QWsS4gIiiCybQe1e'
  },
  {
    id: 26,
    title: 'Call Me Mother',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/CallMe-Mother.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/cSz3TmOmTC0?si=nlWmqJBWqhmcXpmM'
  },
  {
    id: 27,
    title: "I'mPerfect",
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL(/* @vite-ignore */ '../assets/Coming_Soon/I\'m-Perfect.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/2-HhOL5lYGs?si=yuHP29JSylSoeUKL'
  },
  {
    id: 28,
    title: 'Love You So Bad',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/LoveYou-SoBad.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/qSJP7VDCdeQ?si=7-mFioUt-J9EGu0F'
  },
  {
    id: 29,
    title: "Manila's Finest",
    rating: 'PG',
    price: standardPrice,
    poster: new URL(/* @vite-ignore */ '../assets/Coming_Soon/Manila\'s-Finest.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/KqWNzDvwPRo?si=g2cAXy2ZFgsjMeEF'
  },
  {
    id: 30,
    title: 'Rekonek',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Rekonek.jpg', import.meta.url).href,
    trailer: 'https://www.youtube.com/watch?v=dQw4w9WgXcQ'
  },
  {
    id: 31,
    title: 'Shake, Rattle, & Roll: Evil Origins',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/ShakeRattleAndRoll-EvilOrigins.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/A5QQvRaHxDw?si=QIQ3f-egsn22VI3H'
  },
  {
    id: 32,
    title: 'Unmarry',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Unmarry.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/3bLDu6QxdNE?si=p86UJXPTYig_qA6H'
  },
  {
    id: 33,
    title: 'Anaconda',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Anaconda.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/az8M5Mai0X4?si=4QsUGcrAL_P6n0HA'
  },
  {
    id: 34,
    title: 'A Werewolf Boy',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/A-WerewolfBoy.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/8ecTDDCfS9g?si=QU_kJx9GuTZ6yMjj'
  },
  {
    id: 35,
    title: 'The SpongeBob Movie: Search for SquarePants',
    rating: 'G',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/TheSpongeBobMovie-SearchforSquarePants.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/wMfWIu7csjc?si=0jMBo8jxUtkT16U_'
  }
]

const allMovies = {
  'NOW SHOWING': nowShowingMovies,
  'COMING SOON': comingSoonMovies
}

const displayedMovies = computed(() => {
  return allMovies[activeTab.value] || []
})
</script>

<style scoped>
.movies-section {
  padding: 2rem 0;
  background-color: #f9f9f9;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.banner-carousel {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1.5rem;

  .banner-slide img {
    height: 100%;
  }

  .banner-control {
    width: 35px;
    height: 35px;
    font-size: 2rem;
  }
}

.banner-slide {
  flex: 1;
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.banner-slide img {
  width: 100%;
  height: 220px;
  object-fit: cover;
  display: block;
}

.banner-control {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  border: none;
  background-color: rgba(255, 255, 255, 0.9);
  color: #333;
  font-size: 2rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.banner-control:hover {
  background-color: #fff;
}


.tabs {
  display: flex;
  gap: 0;
  border-bottom: 2px solid #ddd;
}

.tab {
  padding: 1rem 2rem;
  background: none;
  border: none;
  color: #666;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
  border-bottom: 3px solid transparent;
  margin-bottom: -2px;
}

.tab.active {
  color: #E63946;
  border-bottom-color: #E63946;
}

.tab:hover {
  color: #E63946;
}

.movies-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

@media (max-width: 768px) {
  .movies-grid {
    grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
    gap: 1rem;
  }

  .tab {
    padding: 0.75rem 1rem;
    font-size: 0.9rem;
  }

  .banner-strip img {
    height: 120px;
  }
}
</style>
