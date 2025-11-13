<template>
  <footer class="footer">
    <div class="container">
      <div class="footer-content">
        <div class="footer-section">
          <h4>Now Showing</h4>
          <ul>
            <li><a href="#">Coming Soon</a></li>
          </ul>
        </div>
        <div class="footer-section">
          <h4>Cinemas</h4>
          <ul>
            <li><a href="#">Events</a></li>
            <li><a href="#">Club</a></li>
          </ul>
        </div>
        <div class="footer-section">
          <h4>Gift Ideas</h4>
          <ul>
            <li><a href="#">Ratings</a></li>
            <li><a href="#">Contact Us</a></li>
          </ul>
        </div>
        <div class="footer-section dpo-section">
          <div class="dpo-badge">
            <div class="dpo-badge-text">DPO/DPS</div>
          </div>
        </div>
      </div>

      <div class="social-links">
        <a href="#" class="social-icon" title="Facebook">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M24 12.073c0-6.627-5.373-12-12-12s-12 5.373-12 12c0 5.99 4.388 10.954 10.125 11.854v-8.385H7.078v-3.47h3.047V9.43c0-3.007 1.792-4.669 4.533-4.669 1.312 0 2.686.235 2.686.235v2.953H15.83c-1.491 0-1.956.925-1.956 1.874v2.25h3.328l-.532 3.47h-2.796v8.385C19.612 23.027 24 18.062 24 12.073z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="Instagram">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <rect x="2" y="2" width="20" height="20" rx="5" ry="5" fill="none" stroke="currentColor" stroke-width="2"/>
            <circle cx="12" cy="12" r="3" fill="none" stroke="currentColor" stroke-width="2"/>
            <circle cx="17" cy="7" r="1" fill="currentColor"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="Twitter">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M23 3a10.9 10.9 0 01-3.14 1.53 4.48 4.48 0 00-7.86 3v1A10.66 10.66 0 013 4s-4 9 5 13a11.64 11.64 0 01-7 2s9 5 20 5a9.5 9.5 0 00-9-5.5c4.75 2.25 7-7 7-7s1.1 5-7 7"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="YouTube">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M23.498 6.186a3.016 3.016 0 00-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 00.502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 002.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 002.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="WhatsApp">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M17.657 10.606a1 1 0 10-1.414-1.414l-6.243 6.243-2.121-2.121a1 1 0 00-1.414 1.414l3.536 3.536a1 1 0 001.414 0l7.242-7.242z"/>
          </svg>
        </a>
        <a href="#" class="social-icon" title="TikTok">
          <svg viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 0C5.373 0 0 5.373 0 12c0 5.303 3.438 9.8 8.206 11.387.6.111.82-.26.82-.577 0-.286-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C3.422 18.07 2.633 17.7 2.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.467-1.334-5.467-5.93 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222 0 1.605-.015 2.896-.015 3.286 0 .319.217.694.825.576C20.565 21.795 24 17.295 24 12c0-6.627-5.373-12-12-12z"/>
          </svg>
        </a>
      </div>

      <div class="footer-bottom">
        <div class="footer-links">
          <a href="#">Terms and Conditions</a>
          <a href="#">Privacy Notice</a>
        </div>
        <div class="copyright">
          Copyright 2025 SM Cinema All Rights Reserved
        </div>
      </div>
    </div>
  </footer>
</template>

<script setup>
</script>

<style scoped>
.footer {
  background-color: #E63946;
  color: white;
  padding: 3rem 0 0;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.footer-content {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 2rem;
  margin-bottom: 2rem;
  padding: 2rem 0;
}

.footer-section h4 {
  font-size: 0.9rem;
  font-weight: 600;
  margin: 0 0 1rem 0;
  text-transform: uppercase;
  letter-spacing: 0.5px;
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
  align-items: center;
  justify-content: flex-end;
}

.dpo-badge {
  width: 80px;
  height: 80px;
  border: 3px solid white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(255, 255, 255, 0.1);
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
  gap: 2rem;
  padding: 2rem 0;
  border-top: 1px solid rgba(255, 255, 255, 0.2);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  margin-bottom: 2rem;
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
  padding: 1.5rem 0;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 1rem;
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
