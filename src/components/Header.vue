<template>
  <header class="header">
    <div class="container">
      <div class="header-content">
        <div class="logo">
          <img :src="logoImage" alt="SM Cinema logo" class="logo-image" />
        </div>
        <nav class="nav">
          <button @click="$emit('navigate', 'home')" :class="{ active: currentPage === 'home' }" class="nav-link">Home</button>
          <button @click="$emit('navigate', 'cinemas')" :class="{ active: currentPage === 'cinemas' }" class="nav-link">Cinemas</button>
          <button @click="$emit('navigate', 'events')" :class="{ active: currentPage === 'events' }" class="nav-link">Events & Experiences</button>
          <button @click="$emit('navigate', 'loyalty')" :class="{ active: currentPage === 'loyalty' }" class="nav-link">Loyalty</button>
          <button @click="$emit('navigate', 'shop')" :class="{ active: currentPage === 'shop' }" class="nav-link">Shop</button>
          <button
            type="button"
            class="nav-link tts-button"
            aria-label="Read page aloud"
            title="Read the current page content aloud"
            @click="readPageAnnouncement"
          >
            <svg class="tts-icon" viewBox="0 0 24 24" aria-hidden="true" focusable="false">
              <path d="M5 9v6h4l5 5V4L9 9H5z" fill="currentColor" />
              <path d="M14 9c1.5 1 1.5 3 0 4" stroke-linecap="round" stroke-width="1.5" fill="none" stroke="currentColor" />
              <path d="M16.5 5c2.5 1.5 2.5 4.5 0 6" stroke-linecap="round" stroke-width="1.5" fill="none" stroke="currentColor" />
            </svg>
            <span class="tts-label">Listen</span>
          </button>
        </nav>
      </div>
    </div>
  </header>
</template>

<script setup>
import { inject } from 'vue'

defineEmits(['navigate'])
const currentPage = inject('currentPage', { value: 'home' })
const logoImage = new URL('../assets/SM_Logo/Logo.png', import.meta.url).href

const getPageText = () => {
  if (typeof document === 'undefined') {
    return ''
  }

  const selectors = ['main', 'section', 'article', '.movies-section', '.content', '.page', '.container']
  const seen = new Set()
  const collected = []

  selectors.forEach((selector) => {
    document.querySelectorAll(selector).forEach((node) => {
      if (!seen.has(node) && node instanceof HTMLElement) {
        seen.add(node)
        const text = node.innerText?.trim()
        if (text) {
          collected.push(text)
        }
      }
    })
  })

  if (!collected.length && document.body) {
    const bodyText = document.body.innerText?.trim()
    if (bodyText) {
      collected.push(bodyText)
    }
  }

  return collected.join(' ').replace(/\s+/g, ' ')
}

const normalizeSpeechText = (text) => {
  return text
    .replace(/₱\s*(\d+)/g, 'price $1 pesos')
    .replace(/\bPHP\s*(\d+)/gi, 'price $1 pesos')
}

let speechInProgress = false

const readPageAnnouncement = () => {
  if (typeof window === 'undefined' || !('speechSynthesis' in window)) {
    return
  }

  if (speechInProgress) {
    window.speechSynthesis.cancel()
    speechInProgress = false
    return
  }

  window.speechSynthesis.cancel()
  const pageText = getPageText()
  const snippet = pageText ? pageText.slice(0, 900) : ''
  const rawMessage = snippet
    ? `Here is an overview of the page: ${snippet}`
    : `Welcome to SM Cinema. You are currently on the ${currentPage?.value || 'home'} page.`

  const message = normalizeSpeechText(rawMessage)

  if (!message.trim()) {
    return
  }

  const utterance = new SpeechSynthesisUtterance(message)
  utterance.rate = 1
  utterance.onend = () => {
    speechInProgress = false
  }
  utterance.oncancel = () => {
    speechInProgress = false
  }

  speechInProgress = true
  window.speechSynthesis.speak(utterance)
}
</script>

<style scoped>
.header {
  background-color: rgb(211, 46, 34);
  padding: 1rem 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.header-content {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 2rem;
}

.logo {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex-shrink: 0;
}

.logo-image {
  width: 100%;
  height: 50px;
  object-fit: contain;
}

.nav {
  display: flex;
  gap: 2rem;
  align-items: center;
}

.nav-link {
  color: white;
  text-decoration: none;
  font-weight: 500;
  font-size: 0.95rem;
  transition: opacity 0.3s ease;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 0;
  font-family: inherit;
  position: relative;
}

.tts-button {
  min-width: 70px;
  display: inline-flex;
  flex-direction: row;
  align-items: center;
  gap: 0.35rem;
  justify-content: center;
  font-size: 0.85rem;
}

.tts-label {
  font-weight: 600;
}

.tts-icon {
  width: 16px;
  height: 16px;
  display: block;
  color: currentColor;
}

.tts-icon path {
  stroke: currentColor;
  stroke-width: 1.5;
  stroke-linecap: round;
  stroke-linejoin: round;
}

.nav-link:hover {
  opacity: 0.8;
}

.nav-link.active {
  font-weight: 700;
  padding-bottom: 0.35rem;
}

.nav-link.active::after {
  content: '';
  position: absolute;
  left: 50%;
  bottom: 0;
  transform: translateX(-50%);
  width: 100%;
  height: 3px;
  border-radius: 999px;
  background: radial-gradient(circle, rgba(255, 255, 255, 0.9), rgba(255, 255, 255, 0.6));
}

@media (max-width: 768px) {
  .nav {
    gap: 1rem;
  }

  .nav-link {
    font-size: 0.85rem;
  }

  .header-content {
    gap: 1rem;
  }
}
</style>
