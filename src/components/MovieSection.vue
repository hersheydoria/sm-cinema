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
const standardPrice = 'Starting ₱330'
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
    trailer: 'https://youtu.be/Mtou4LuxFrg?si=8-vsDNPXYbHdaUHQ',
    synopsis:
      'A mother, who loves watching KDramas, decides to stop her chemo treatment after losing hope of getting better. Her only dream before dying is to meet her ultimate KDrama idol, who is visiting Manila for a series of events. Her four children agree to work together with the condition that once their mother meets their idol, she will undergo therapy before her time runs out. Run Time: 1 hours 47 minutes.'
  },
  {
    id: 2,
    title: 'Wicked: For Good',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/The-Wicked.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/R2Xubj7lazE?si=7DfsSII3eHKA5cSf',
    synopsis:
      'Elphaba, the future Wicked Witch of the West and her relationship with Glinda, the Good Witch of the North. The second of a two-part feature film adaptation of the Broadway musical. Run Time: 2 hours 18 minutes.'
  },
  {
    id: 3,
    title: "Now You See Me: Now You Don't",
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/NowYouSeeMe-NowYouDont.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/-E3lMRx7HRQ?si=syCXRQHpMtLEi63W',
    synopsis:
      'A diamond heist reunites retired Horsemen illusionists with new performers Greenblatt, Smith and Sessa as they target dangerous criminals. Run Time: 1 hours 52 minutes.'
  },
  {
    id: 4,
    title: 'Zootopia 2',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Zootopia-2.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/BjkIOU5PhyQ?si=gwTO3__5o-OQExgA',
    synopsis:
      'In Walt Disney Animation Studios’ “Zootopia 2,” detectives Judy Hopps and Nick Wilde find themselves on the twisting trail of a mysterious reptile who arrives in Zootopia and turns the animal metropolis upside down. To crack the case, Judy and Nick must go undercover to unexpected new parts of town, where their growing partnership is tested like never before. Run Time: 1 hours 47 minutes.'
  },
  {
    id: 5,
    title: 'Tha Rae: The Exorcist',
    rating: 'M',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/TheRae-TheExorcist.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/TshZVPCrtF0?si=TYIbw9AX8PcRGc1L',
    synopsis:
      "In Thailand's largest Catholic village, Tha Rae, a demon returns after forty years, possessing a former priest and spreading terror. Traditional exorcism fails, forcing a by-the-book priest and a rogue shaman to join forces. Run Time: 1 hours 58 minutes."
  },
  {
    id: 6,
    title: 'Salvageland',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Salvage-Land.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/Y1ClNzed-g4?si=AKL8BHQ3o_SJbuB5',
    synopsis:
      '"Salvageland” is a gritty neo-Western thriller about moral reckoning and the price of justice in a land scorched by violence and neglect—where men are shaped by silence, fear, and the unforgiving environment they’ve learned to survive. Run Time: 1 hours 30 minutes.'
  },
  {
    id: 7,
    title: 'SEVENTEEN WORLD TOUR [NEW_] IN JAPAN: LIVE VIEWING',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/SEVENTEEN_WORLD_TOUR_NEW_IN_JAPAN_LIVE_VIEWING.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/EoGFOvMTBhM?si=k5LS-DyRLrwxilWj',
    synopsis:
      'Celebrating their 10th anniversary, SEVENTEEN embarks on a monumental new journey with their world tour [NEW_], symbolizing a fresh start and endless possibilities ahead. Known as a performance powerhouse, SEVENTEEN delivers dynamic stages, reimagined choreography, and solo performances that spotlight each member’s growth and artistry. From Incheon to North America and across Asia, the journey culminates in the Nagoya concert, broadcast live to cinemas worldwide on November 29. Experience SEVENTEEN’s passion, unity, and energy; on the big screen, together with fans around the world. Run Time: 3 hours 30 minutes.'
  },
  {
    id: 8,
    title: "KMJS' Gabi Ng Lagim: The Movie",
    rating: 'PG',
    price: standardPrice,
    poster: new URL("../assets/Now_Showing/KMJS'GabiNgLagim-TheMovie.jpg", import.meta.url).href,
    trailer: 'https://youtu.be/IeW72gqVPhE?si=NE8VXDjfabfFjZjh',
    synopsis:
      'Three chilling tales based on real stories expose the haunting depths of faith, fear, and the unknown — from a seafarer trapped on a cursed voyage, to an island plagued by a flesh-eating legend, and a young woman’s battle against a legion of demons that pushes the limits of faith and sanity. Run Time: 1 hours 57 minutes.'
  },
  {
    id: 9,
    title: 'Keeper',
    rating: 'M',
    price: standardPrice,
    poster: new URL('../assets/Now_Showing/Keeper.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/cwpusY785l4?si=QMgqIFKhL_Q7JQE4',
    synopsis:
      'A romantic anniversary trip to a secluded cabin turns sinister when a dark presence reveals itself, forcing a couple to confront the property\'s haunting past. Run Time: 1 hours 41 minutes.'
  }
]

const comingSoonMovies = [
  {
    id: 1,
    title: 'Ang Happy Homes Ni Diane Hilario',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/AngHappyHomes-NiDianeHilario.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/QMUkKo16T7I?si=DAHM5wZeNGmdLcxt',
    synopsis:
      'A story of ordinary, happy people living in a tenement is suddenly shaken by a string of conspicuous deaths. Unfortunate and unrelated deaths continue, and they think a killer is living among them. Run Time: 1 hours 40 minutes.'
  },
  {
    id: 2,
    title: 'Eternity',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Eternity.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/irXTps1REHU?si=C7YjzimOIUXaqWpM',
    synopsis:
      'In an afterlife where souls have one week to decide where to spend eternity, Joan is faced with the impossible choice between the man she spent her life with and her first love, who died young and has waited decades for her to arrive. Run Time: 1 hours 53 minutes.'
  },
  {
    id: 3,
    title: "Five Nights at Freddy's 2",
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/FiveNightsAtFreddys2.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/dSDpoobO6yM?si=G89UENqWN0mL2kD7',
    synopsis:
      'Anyone can survive five nights. This time, there will be no second chances. Run Time: 1 hours 43 minutes.'
  },
  {
    id: 4,
    title: 'Jackstone 5',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Jackstone-5.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/KmvqpgI-g1c?si=_5j6kzXNy5AERcvj',
    synopsis:
      'The story of a lifelong friendship between five gay men who reunite after many years apart. It’s a story that many adults can relate to—when life gets busy and you hardly see your friends anymore, but once you do, it feels like no time has passed. You start reminiscing about the good old days, the fun moments, and all the memories you shared together. Run Time: 1 hours 43 minutes.'
  },
  {
    id: 5,
    title: 'JUJUTSU KAISEN: Shibuya Incident×The Culling Game',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/JUJUTSUKAISEN-ShibuyaIncident×TheCullingGame.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/-wkgtm0nsOg?si=THY_4QYHs_HbmPxe',
    synopsis:
      'A veil abruptly descends over the busy Shibuya area amid the bustling Halloween crowds, trapping countless civilians inside. In the aftermath, ten colonies across Japan are transformed into dens of curses. Run Time: 1 hours 27 minutes.'
  },
  {
    id: 6,
    title: 'MONSTA X: CONNECT X IN CINEMAS',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/MONSTAX-CONNECTXINCINEMAS.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/eodDzdSNX8U?si=3h2Vvm5S_-djrzRU',
    synopsis:
      'In July 2025, MONSTA X set the KSPO DOME ablaze over three unforgettable days with their show 2025 MONSTA X CONNECT X. From the powerful live band session and a setlist packed with spectacular performances, to never-before-seen behind-the-scenes footage of the preparation process, and exclusive, heartfelt interviews where the members reflect on their intense 10-year journey—this film captures it all. A chronicle of a decade-long story written together by MONSTA X and MONBEBE. Run Time: 1 hours 57 minutes.'
  },
  {
    id: 7,
    title: 'Nasaan si Hesus?',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Nasaansi-Hesus.jpg', import.meta.url).href,
    synopsis:
      'The lives of various individuals in the village of Sto. Nino intertwine as they begin to question the presence and meaning of God in their lives. Roger, his wife Brenda, and their daughter Cindy seem to be a model family, but their faith is questioned when sin and its repercussions begin to affect their home. The Parish Priest, Fr. Carlo, realizes he is prone to anger and temptation, putting to doubt his own spirituality. Sister Remedios, a nun, makes it her mission to help a group of streetchildren, despite the reluctance of the novice Sister Alma. Single mother Lucy’s troubles escalate as she turns to drugs to help support herself and her baby. And other residents have to deal with their selfish employers like Mrs. Varona or opportunistic politicians like Congressman Caramba. In the end, each one tries to find the right path to mend their lives. Run Time: 1 hours 30 minutes.'
  },
  {
    id: 8,
    title: "The Carpenter's Son",
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-CarpentersSon.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/aszdL5VFS6Q?si=2wZD6hdtuM-7yUrM',
    synopsis:
      "Family hiding in Roman Egypt. Son known as 'the Boy' doubts guardian 'the Carpenter', rebelling with mysterious powers. As he uses abilities, they face natural and divine horrors. Run Time: 1 hours 34 minutes."
  },
  {
    id: 9,
    title: 'The Ghost Village',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-GhostVillage.jpg', import.meta.url).href,
      synopsis:
        "In Kogalok Village, young men are mysteriously found dead in a field once known for lovers' meetings. Village chief Pong investigates and finds no signs of struggle. Only stiff bodies with wide eyes. The villagers suspect a ghost, so Pong calls a shaman for help. They discover the body of a murdered LGBTQ+ woman and realize her revengeful spirit is behind the deaths. Now, Pong must find the killer to stop the haunting and bring peace. Can he solve the mystery? Run Time: 1 hours 39 minutes."
  },
  {
    id: 10,
    title: 'Light of the World',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/LightOf-TheWorld.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/rGd9zO_lvU4?si=NrIv0hLaygWlbSp8',
    synopsis:
      "Follows Jesus' life from ministry beginnings through crucifixion, resurrection, as seen through Apostle John's eyes. Run Time: 1 hours 30 minutes."
  },
  {
    id: 11,
    title: 'Scarlet',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Scarlet.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/XNar7yF-pf4?si=vQc1Pf0zOXykMg2_',
    synopsis:
      "A murdered princess awakens in a realm between life and death. Racing against time in a world of chaos, she must defeat her father's killer and reach a mythical sanctuary before her soul vanishes forever. Run Time: 1 hours 51 minutes."
  },
  {
    id: 12,
    title: 'The Incredible Shrinking Man',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-IncredibleShrinkingMan.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/dX1E-9jE4bs?si=tht11onjNHTdCRwo',
    synopsis:
      'Shipbuilder inexplicably shrinks, gets trapped in basement, few inches tall. Must fight for survival in now-hostile ordinary household environment. Sci-fi premise, no explanation given for shrinking phenomenon. Run Time: 1 hours 38 minutes.'
  },
  {
    id: 13,
    title: 'The Shining 45th Year Anniversary',
    rating: 'R',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/The-Shining45thYearAnniversary.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/RmQPBzJKxcw?si=CeWP_eHv4yfz8o6l',
    synopsis:
      'A family heads to an isolated hotel for the winter, where a sinister presence influences the father into violence. At the same time, his psychic son sees horrifying forebodings from both the past and the future. Run Time: 2 hours 25 minutes.'
  },
  {
    id: 14,
    title: 'Avatar: Fire and Ash',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Avatar-FireandAsh.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/zO9GTJotxEQ?si=VjB8Tc7Scc8JEm4z',
    synopsis:
      "Jake and Neytiri's family grapples with grief after Neteyam's death, encountering a new, aggressive Na'vi tribe, the Ash People, who are led by the fiery Varang, as the conflict on Pandora escalates and a new moral focus emerges. Run Time: 3 hours 15 minutes."
  },
  {
    id: 15,
    title: 'Bar Boys: After School',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/BarBoys-AfterSchool.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/K4fQCnApsW8?si=QWsS4gIiiCybQe1e',
    synopsis:
      '10 years after law school, Torran, Erik, Chris, and Josh face mid-life struggles, burnout, heartbreak, and lost purpose. When they learn that their former mentor Justice Hernandez is gravely ill, they reunite to care for her. Through this, they confront their life choices, rediscover their ideas, and find new meanings as mentors to the next generation. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 16,
    title: 'Call Me Mother',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/CallMe-Mother.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/cSz3TmOmTC0?si=nlWmqJBWqhmcXpmM',
    synopsis:
      'Twinkle is a single queer mother whose dream of officially adopting her son gets shaken when the boy’s biological mother, Mara, unexpectedly reappears. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 17,
    title: "I'mPerfect",
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL(/* @vite-ignore */ '../assets/Coming_Soon/Im-Perfect.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/2-HhOL5lYGs?si=yuHP29JSylSoeUKL',
    synopsis:
      'The story is about Jiro and Jessica, two adults with Down syndrome, who meet, fall in love, and discover the true meaning of freedom and acceptance. As they face life challenges together, their bond grows deeper. But when Jessica falls ill and passes away, Jiro is left cherishing their moments and memories, a reminder of love, acceptance, and being true to oneself. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 18,
    title: 'Love You So Bad',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/LoveYou-SoBad.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/qSJP7VDCdeQ?si=7-mFioUt-J9EGu0F',
    synopsis:
      'Sassy, confident and unapologetically herself, college senior Savannah is determined to live life on her own terms. She finds herself torn between two different men: bad boy and chick magnet L.A. who fuels her wild side, and principled and ambitious Vic who challenges her to grow. As old wounds resurface and difficult choices confront her, she must decide who she really wants to be and who she dares to love. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 19,
    title: "Manila's Finest",
    rating: 'PG',
    price: standardPrice,
    poster: new URL(/* @vite-ignore */ '../assets/Coming_Soon/Manilas-Finest.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/KqWNzDvwPRo?si=g2cAXy2ZFgsjMeEF',
    synopsis:
      'Policemen Homer, Conrad, and Billy are consumed by the murder case of troublemaking teenagers in the slums during the first quarter storm in the 1970s. The disappearance of students increases as the gap between the rich and the poor widens. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 20,
    title: 'Rekonek',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Rekonek.jpg', import.meta.url).href,
    synopsis:
      'In a multi-genre and multi-narrative movie, Filipinos struggle to enjoy Christmas without the internet. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 21,
    title: 'Shake, Rattle, & Roll: Evil Origins',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/ShakeRattleAndRoll-EvilOrigins.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/A5QQvRaHxDw?si=QIQ3f-egsn22VI3H',
    synopsis:
      'The "Shake, Rattle, & Roll" franchise takes a unique turn as its three stories will be set in different time periods. In 1775, four women are murdered when a dark force escapes from the strange chest they received from a Spanish ship. In 2025, a Halloween party in an abandoned hotel turns into a horror house scenario as a mysterious killer looms over the partygoers. In 2050, an Aswang rules the Philippines, and a man named Hunter discovers his family may be linked to the chaos. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 22,
    title: 'Unmarry',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Unmarry.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/3bLDu6QxdNE?si=p86UJXPTYig_qA6H',
    synopsis:
      'Celine seeks an annulment from her husband, while fighting for the custody of their daughters. Meanwhile, Ivan struggles to save his marriage and their son. Their paths cross at the law office of a lawyer and YouTube host of "Walang Butas Ang Batas". Amid legal battles and chance encounters, Celine and Ivan form an unexpected friendship that helps them heal and rediscover their home. As they grow close, the possibility of finding a new chance at love presents itself. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 23,
    title: 'Anaconda',
    rating: 'PG-13',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/Anaconda.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/az8M5Mai0X4?si=4QsUGcrAL_P6n0HA',
    synopsis:
      'Doug and Griff have been best friends since they were kids, and have always dreamed of remaking their all-time favorite movie: the cinematic "classic" Anaconda. When a midlife crisis pushes them to finally go for it, they head deep into the Amazon to start filming. But things get real when an actual giant anaconda appears, turning their comically chaotic movie set into a deadly situation. The movie they’re dying to make? It might just get them killed... Run Time: 1 hours 40 minutes.'
  },
  {
    id: 24,
    title: 'A Werewolf Boy',
    rating: 'PG',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/A-WerewolfBoy.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/8ecTDDCfS9g?si=QU_kJx9GuTZ6yMjj',
    synopsis:
      'The story centers on a city girl who moves back to the countryside and forms a unique bond with a mysterious, orphan boy. Run Time: 2 hours 00 minutes.'
  },
  {
    id: 25,
    title: 'The SpongeBob Movie: Search for SquarePants',
    rating: 'G',
    price: standardPrice,
    poster: new URL('../assets/Coming_Soon/TheSpongeBobMovie-SearchforSquarePants.jpg', import.meta.url).href,
    trailer: 'https://youtu.be/wMfWIu7csjc?si=0jMBo8jxUtkT16U_',
    synopsis:
      'SpongeBob and his Bikini Bottom friends set sail in their biggest, all-new, can’t miss cinematic event ever… The SpongeBob Movie: Search for SquarePants. Desperate to be a big guy, SpongeBob sets out to prove his bravery to Mr. Krabs by following The Flying Dutchman – a mysterious swashbuckling ghost pirate – on a seafaring comedy-adventure that takes him to the deepest depths of the deep sea, where no Sponge has gone before. Run Time: 1 hours 36 minutes.'
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
  width: 28px;
  height: 28px;
  border-radius: 50%;
  border: none;
  background-color: rgba(255, 255, 255, 0.9);
  color: #333;
  font-size: 1.5rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.banner-control:hover {
  background-color: #fff;
}


.section-header {
  margin-bottom: 1.25rem;
}

.tabs {
  display: flex;
  gap: 0.75rem;
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
  margin-top: 1.25rem;
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
