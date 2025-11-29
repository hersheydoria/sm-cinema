<template>
  <footer class="footer">
    <div class="container">
    <div class="social-links">
        <a href="#" class="social-icon" title="Facebook" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="Instagram" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor">
            <rect x="2" y="2" width="20" height="20" rx="5" ry="5" fill="none" stroke="currentColor" stroke-width="2"/>
            <circle cx="12" cy="12" r="3" fill="none" stroke="currentColor" stroke-width="2"/>
            <circle cx="17" cy="7" r="1" fill="currentColor"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="Twitter" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M23.643 4.937a9.702 9.702 0 01-2.828.773 4.932 4.932 0 002.165-2.724 9.864 9.864 0 01-3.127 1.195 4.918 4.918 0 00-8.384 4.48A13.962 13.962 0 011.671 4.149a4.917 4.917 0 001.523 6.567 4.897 4.897 0 01-2.23-.616 4.916 4.916 0 003.946 3.822 4.932 4.932 0 01-2.224.086 4.92 4.92 0 004.6 3.421 9.867 9.867 0 01-6.102 2.105c-.395 0-.787-.023-1.17-.068a13.945 13.945 0 007.557 2.212c9.054 0 14.002-7.496 14.002-13.985 0-.21-.005-.42-.015-.63A9.936 9.936 0 0023.643 4.937z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="YouTube" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M23.498 6.186a3.016 3.016 0 00-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 00.502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 002.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 002.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="Viber" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
            <path d="M11.4 0C9.473.028 5.333.344 3.02 2.467 1.302 4.187.696 6.7.633 9.817.57 12.933.488 18.776 6.12 20.36h.003l-.004 2.416s-.037.977.61 1.177c.777.242 1.234-.5 1.98-1.302.407-.44.972-1.084 1.397-1.58 3.85.326 6.812-.416 7.15-.525.776-.252 5.176-.816 5.892-6.657.74-6.02-.36-9.83-2.34-11.546-.596-.55-3.006-2.3-8.375-2.323 0 0-.395-.025-1.037-.017zm.058 1.693c.545-.004.88.017.88.017 4.542.02 6.717 1.388 7.222 1.846 1.675 1.435 2.53 4.868 1.906 9.897v.002c-.604 4.878-4.174 5.184-4.832 5.395-.28.09-2.882.737-6.153.524 0 0-2.436 2.94-3.197 3.704-.12.12-.26.167-.352.144-.13-.033-.166-.188-.165-.414l.02-4.018c-4.762-1.32-4.485-6.292-4.43-8.895.054-2.604.543-4.738 1.996-6.173 1.96-1.773 5.474-2.018 7.11-2.03zm.38 2.602c-.167 0-.303.135-.304.302 0 .167.133.303.3.305 1.624.01 2.946.537 4.028 1.592 1.073 1.046 1.62 2.468 1.633 4.334.002.167.14.3.307.3.166-.002.3-.138.3-.304-.014-1.984-.618-3.596-1.816-4.764-1.19-1.16-2.692-1.753-4.447-1.765zm-3.96.695c-.19-.032-.4.005-.616.117l-.01.002c-.43.247-.816.562-1.146.932-.002.004-.006.004-.008.008-.267.323-.42.638-.46.948-.008.046-.01.093-.007.14 0 .136.022.27.065.4l.013.01c.135.48.473 1.276 1.205 2.604.42.768.903 1.5 1.446 2.186.27.344.56.673.87.984l.132.132c.31.308.64.6.984.87.686.543 1.418 1.027 2.186 1.447 1.328.733 2.126 1.07 2.604 1.206l.01.014c.13.042.265.064.402.063.046.002.092 0 .138-.008.31-.036.627-.19.948-.46.004 0 .003-.002.008-.005.37-.33.683-.72.93-1.148l.003-.01c.225-.432.15-.842-.18-1.12-.004 0-.698-.58-1.037-.83-.36-.255-.73-.492-1.113-.71-.51-.285-1.032-.106-1.248.174l-.447.564c-.23.283-.657.246-.657.246-3.12-.796-3.955-3.955-3.955-3.955s-.037-.426.248-.656l.563-.448c.277-.215.456-.737.17-1.248-.217-.383-.454-.756-.71-1.115-.25-.34-.826-1.033-.83-1.035-.137-.165-.31-.265-.502-.297zm4.49.88c-.158.002-.29.124-.3.282-.01.167.115.312.282.324 1.16.085 2.017.466 2.645 1.15.63.688.93 1.524.906 2.57-.002.168.13.306.3.31.166.003.305-.13.31-.297.025-1.175-.334-2.193-1.067-2.994-.74-.81-1.777-1.253-3.05-1.346h-.024zm.463 1.63c-.16.002-.29.127-.3.287-.008.167.12.31.288.32.523.028.875.175 1.113.422.24.245.388.62.416 1.164.01.167.15.295.318.287.167-.008.295-.15.287-.317-.03-.644-.215-1.178-.58-1.557-.367-.378-.893-.574-1.52-.607h-.018z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="TikTok" @click.prevent>
          <svg viewBox="0 0 24 24" fill="currentColor" aria-hidden="true">
            <path d="M12.525.02c1.31-.02 2.61-.01 3.91-.02.08 1.53.63 3.09 1.75 4.17 1.12 1.11 2.7 1.62 4.24 1.79v4.03c-1.44-.05-2.89-.35-4.2-.97-.57-.26-1.1-.59-1.62-.93-.01 2.92.01 5.84-.02 8.75-.08 1.4-.54 2.79-1.35 3.94-1.31 1.92-3.58 3.17-5.91 3.21-1.43.08-2.86-.31-4.08-1.03-2.02-1.19-3.44-3.37-3.65-5.71-.02-.5-.03-1-.01-1.49.18-1.9 1.12-3.72 2.58-4.96 1.66-1.44 3.98-2.13 6.15-1.72.02 1.48-.04 2.96-.04 4.44-.99-.32-2.15-.23-3.02.37-.63.41-1.11 1.04-1.36 1.75-.21.51-.15 1.07-.14 1.61.24 1.64 1.82 3.02 3.5 2.87 1.12-.01 2.19-.66 2.77-1.61.19-.33.4-.67.41-1.06.1-1.79.06-3.57.07-5.36.01-4.03-.01-8.05.02-12.07z"/>
          </svg>
        </a>
      </div>
      <div class="footer-content">
        <div class="footer-section">
          <ul>
            <li><a href="#" @click.prevent="navigateTo('home', 'comingSoon')">Coming Soon</a></li>
            <li><a href="#" @click.prevent="navigateTo('home', 'nowShowing')">Now Showing</a></li>
          </ul>
        </div>
        <div class="footer-section">
          <ul>
            <li><a href="#" @click.prevent="navigateTo('cinemas')">Cinemas</a></li>
            <li><a href="#" @click.prevent="navigateTo('events')">Events</a></li>
            <li><a href="#" @click.prevent="navigateTo('loyalty')">Club</a></li>
          </ul>
        </div>
        <div class="footer-section">
          <ul>
            <li><a href="#" @click.prevent="navigateTo('shop')">Gift Ideas</a></li>
            <li><a href="#" @click.prevent="navigateTo('ratings')">Ratings</a></li>
            <li><a href="#" @click.prevent="navigateTo('contact')">Contact Us</a></li>
          </ul>
        </div>
        <div class="footer-section dpo-section">
          <div class="dpo-badge">
            <img :src="dpoLogo" alt="DPO badge" class="logo-image" />
          </div>
        </div>
      </div>

      <div class="footer-bottom">
        <div class="footer-links">
          <a href="#" @click.prevent="handleTerms">Terms and Conditions</a>
          <a href="#" @click.prevent="handlePrivacy">Privacy Notice</a>
        </div>
        <div class="copyright">
          Copyright 2021 SM Cinema All Rights Reserved
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { inject } from 'vue'

const currentPage = inject('currentPage', { value: 'home' })
const dpoLogo = new URL('../assets/footer/dpo.png', import.meta.url).href

const navigateTo = (page, filter = null) => {
  currentPage.value = page
  if (filter) {
    localStorage.setItem('movieFilter', filter)
  }
}

const handleTerms = () => {
  alert('Terms and Conditions coming soon!')
}

const handlePrivacy = () => {
  alert('Privacy Notice coming soon!')
}
</script>

<style scoped>
.footer {
  background-color: rgb(211, 46, 34);
  color: white;
  padding: 2rem 0 1rem;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1.5rem;
}

.footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  padding: 1rem 0;
}

.footer-section h4 {
   display: flex;
   align-items: center;
   justify-content: flex-end;
   letter-spacing: 0.5px;
   width: 80px;
   height: 100px;
}
 
.footer-section ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

.footer-section li {
  margin-bottom: 0.5rem;
}

.footer-section a {
  color: white;
  text-decoration: none;
  font-size: 0.9rem;
  transition: opacity 0.3s ease;
}

.footer-section a:hover {
  opacity: 0.8;
}

.dpo-section {
  display: flex;
  align-items: top;
  justify-content: flex-end;
  margin-top: -100px;
}

.dpo-badge {
  width: 80%;
  height: 80%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.dpo-badge .logo-image {
  max-width: 80%;
  max-height: 80%;
  object-fit: contain;
}

.dpo-badge-text {
  text-align: center;
  font-size: 0.75rem;
  font-weight: 700;
  line-height: 1.2;
}

.social-links {
  display: flex;
  justify-content: center;
  gap: 1.5rem;
  padding: 1rem 0;
  margin-bottom: 1rem;
}

.social-icon {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  color: white;
  transition: opacity 0.3s ease;
}

.social-icon:hover {
  opacity: 0.8;
}

.social-icon svg {
  width: 100%;
  height: 100%;
}

.footer-bottom {
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  padding: 1rem 0 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 0.75rem;
  font-size: 0.85rem;
}

.footer-links {
  display: flex;
  gap: 1.5rem;
}

.footer-links a {
  color: white;
  text-decoration: none;
  transition: opacity 0.3s ease;
}

.footer-links a:hover {
  opacity: 0.8;
}

.copyright {
  text-align: right;
}

@media (max-width: 768px) {
  .footer-content {
    grid-template-columns: repeat(2, 1fr);
    gap: 1.5rem;
  }

  .dpo-section {
    justify-content: center;
    grid-column: 1 / -1;
  }

  .footer-bottom {
    flex-direction: column;
    text-align: center;
  }

  .copyright {
    text-align: center;
  }

  .social-links {
    gap: 1rem;
  }
}
</style>
