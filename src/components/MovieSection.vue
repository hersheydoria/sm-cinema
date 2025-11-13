<template>
  <section class="movies-section">
    <div class="container">
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
        />
      </div>

      <div class="view-all">
        <button class="view-all-button">View All Movies</button>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'
import MovieCard from './MovieCard.vue'

const activeTab = ref('NOW SHOWING')
const tabs = ['NOW SHOWING', 'COMING SOON']

const allMovies = {
  'NOW SHOWING': [
    {
      id: 1,
      title: 'Quezon',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BNDA5MjBjYTItOTMwZS00NWY4LWIwNTItMzk4OTY1MjE3NTc4XkEyXkFqcGc@._V1_SX300.jpg'
    },
    {
      id: 2,
      title: 'Meet, Greet & Bye',
      rating: 'G',
      poster: 'https://m.media-amazon.com/images/M/MV5BMjAxMzkyMzAtNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 3,
      title: 'Now You See Me: Now You Don\'t',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BYTA0OGY0ZDItYzQ0Yy00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 4,
      title: 'Predator: Badlands',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BNDA5MTAwZTItNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 5,
      title: 'The Running Man',
      rating: 'M',
      poster: 'https://m.media-amazon.com/images/M/MV5BNjg4OTc2YTItNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 6,
      title: 'J-Hope Tour \'HOPE ON THE STAGE\' THE MOVIE',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMjAzNDU0MDItNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 7,
      title: 'Lakambini, Gregoria De Jesus',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMjEzNDU0MDItNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 8,
      title: 'Jujutsu Kaisen: Zero Revival',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMjIzNDU0MDItNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    }
  ],
  'COMING SOON': [
    {
      id: 9,
      title: 'Elio',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BOTA5MzgyMDU2OF5BMl5BanBnXkFtZTgwODg5ODc5NzM@._V1_SX300.jpg'
    },
    {
      id: 10,
      title: 'Finding Santos',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMjAxMzkyMzAtNDYyNi00NzMyLWE4NTEtNmE1NDI2ZmM3MzljXkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 11,
      title: 'Wicked: For Good',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BOWMwYjYzYmMtMWQ2Ni00NWUwLTg2MzAtYzkzMDBiZDIwOTMwXkEyXkFqcGc@._V1_SX300.jpg'
    },
    {
      id: 12,
      title: 'Wildcat',
      rating: 'M',
      poster: 'https://m.media-amazon.com/images/M/MV5BNDAxMmM3YTItNjY3Yi00ZjY5LTg0NWUtZDZmMDc1MDBiYWU1XkEyXkFqcGdeQXVyMTMxODk2OTU@._V1_SX300.jpg'
    },
    {
      id: 13,
      title: 'Keeper',
      rating: 'M',
      poster: 'https://m.media-amazon.com/images/M/MV5BOTYyMDI1MjctMzY4OC00ZTc1LWE4ZDAtMTdkNGE5YzI0M2YxXkEyXkFqcGdeQXVyNjExODE0OTU@._V1_SX300.jpg'
    },
    {
      id: 14,
      title: 'KMJs: Gabi Ng Lagim: The Movie',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BYzMyOTczODQtMmRjNC00OTZjLWJmOTctMGQwODgyNTc3MDQ5XkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 15,
      title: 'Vaguelad',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZGIxYTMxZDgtNzU0Yi00MzQ1LTk5ZmYtMTQ2MzVkYjI5ZWIwXkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 16,
      title: 'Tka Raa: The Eventis',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZDYzODczZjQtN2M3YS00YjhkLWE1Y2MtMmEwNDMyMWQ3MmM4XkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 17,
      title: 'Zootopia 3',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZTg4ZDZlNTItZjdjYi00YmM0LWI4YTItNzYzYThjMDQxOGY2XkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 18,
      title: 'Godzilla x Kong: New Empire',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZGMwMjMwNDAtYmMzZi00ZTMxLTk2NDUtYjhkODY1ZGU1MjdkXkEyXkFqcGdeQXVyODE5NzE3OTE@._V1_SX300.jpg'
    },
    {
      id: 19,
      title: 'Eternity',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMTQzMGM5YzItYzQzNy00MzgyLWJjMWUtNDgxMmI1ZjFkNjYwXkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 20,
      title: 'Nexus: Connect in Cinemas',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZWNlYTNkOTItNmYwMS00ZDY2LWJmYjUtYjBjZWEzZTI0MDZiXkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 21,
      title: 'Light Of The World',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BMTU5ODkzMDI0OV5BMl5BanBnXkFtZTgwODQ3NTA4NjE@._V1_SX300.jpg'
    },
    {
      id: 22,
      title: 'The Incredibles: Shaking You',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BOWY4OTBhODItNTJlZi00YTQ5LWJlMzEtMWY4YTg0NmU1ZjRjXkEyXkFqcGdeQXVyMzM4MjM2Nzg@._V1_SX300.jpg'
    },
    {
      id: 23,
      title: 'Avatar: Fire And Ash',
      rating: 'PG',
      poster: 'https://m.media-amazon.com/images/M/MV5BZDYxY2I1OGMtN2Y4MS00ZmU1LTgyNDAtODA0MzAyYjI0N2Y2XkEyXkFqcGc@._V1_SX300.jpg'
    }
  ]
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

.section-header {
  margin-bottom: 2rem;
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

.view-all {
  display: flex;
  justify-content: center;
  padding: 1rem 0;
}

.view-all-button {
  padding: 0.75rem 2rem;
  background-color: #E63946;
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.view-all-button:hover {
  background-color: #D62828;
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
}
</style>
