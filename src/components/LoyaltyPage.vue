<template>
  <section class="loyalty-section">
    <!-- Pick Tickets Modal Component -->
    <PickTicketsModal ref="pickTicketsModal" />

    <div class="container">

      <!-- Pick Tickets Section -->
      <div class="pick-tickets">
        <div class="pick-tickets-content">
          <h2>PICK TICKETS</h2>
          <p>Browse and Book sessions easily. Start with your choice of Movie, Cinema, Show Type or Time.</p>
          
          <div class="pick-buttons">
            <button class="pick-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('MOVIES')">PICK A<br>MOVIE</button>
            <button class="pick-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('CINEMA')">PICK A<br>CINEMA</button>
            <button class="pick-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('SHOW TYPE')">PICK A<br>SHOW TYPE</button>
            <button class="pick-btn" @click="$refs.pickTicketsModal.openPickMoviesModal('TIME')">PICK A<br>TIME</button>
          </div>
        </div>

        <div
          class="imax-banner"
          @mouseenter="pauseBannerRotation"
          @mouseleave="resumeBannerRotation"
        >
          <img :src="currentBanner" alt="Cinema banner" class="imax-image">
          <div class="imax-content">
            <div class="imax-logo"></div>
          </div>
        </div>
      </div>

      <!-- Loyalty Banner Section -->
      <div v-if="!showDirectorsClubForm" class="loyalty-banner-section">
        <div class="loyalty-banner">
          <img :src="loyaltyBannerImage" alt="Loyalty banner" class="loyalty-banner-image" />
        </div>
      </div>

      <!-- Membership Details Section (shown when sign up is clicked) -->
      <div v-if="showMembershipDetails && !showDirectorsClubForm" class="membership-details">
        <button class="back-btn" @click="goBack">← Back</button>
        
        <div class="membership-content">
          <div class="membership-left">
            <h2>SM Cinema Club Basic Membership</h2>
            <p class="membership-description">Join at this membership level and receive great benefits!</p>
            
            <div class="membership-description-box">
              <p>Simply sign up and start earning points every time you watch movies!</p>
              <h4>What you get:</h4>
              <ul class="benefits-list">
                <li>₱ 1.00 signup fee instantly gets you 1 Cinema Point</li>
                <li>Earning of points: 1 point for every <strong>₱50 spend</strong></li>
              </ul>
            </div>
          </div>

          <div class="membership-right">
            <div class="offer-card">
              <div class="offer-header">DIRECTOR'S CLUB</div>
              <h3>FRESHLY<br>POPPED FOR YOU</h3>
              <p class="offer-description">Director's Club Cinema patrons will be served complimentary fresh popcorn with your choice of flavor inside the cinema.</p>
              <p class="offer-price">₱1,200.00</p>
              <button class="signup-now-btn" @click="openDirectorsClubForm">Sign Up Now</button>
              <img src="https://via.placeholder.com/250x200?text=Popcorn" alt="Popcorn" class="offer-image">
            </div>
          </div>
        </div>
      </div>

      <!-- Get In On The Action Section (hidden when membership details shown) -->
      <div v-else-if="!showDirectorsClubForm" class="action-section">
        <div class="action-left">
          <h3>Get in on the action!</h3>
          <h4>Find out more.</h4>
          <button class="signup-btn" @click="openSignUpForm">Sign Up</button>
        </div>

        <div class="club-login">
          <h3>SM CINEMA CLUB</h3>
          <form @submit.prevent="handleLogin" class="login-form">
            <input 
              type="email" 
              v-model="loginData.email"
              placeholder="Email" 
              required
              class="form-input"
            />
            <input 
              type="password" 
              v-model="loginData.password"
              required
              class="form-input"
            />
            <div class="form-links">
              <a href="#" class="forgot-link">Forgot Password?</a>
              <button type="submit" class="sign-in-btn">Sign In</button>
            </div>
          </form>
        </div>
      </div>

      <!-- Director's Club Sign Up Form Section -->
      <div v-if="showDirectorsClubForm" class="signup-form-section">
        <button class="back-btn" @click="goBackToMembership">← Back</button>
        
        <div class="form-wrapper">
          <h2>SM Club Premium Member - ₱1,200.00</h2>
          
          <form @submit.prevent="handleDirectorsClubSignUp" class="membership-form">
            <!-- Personal Details Section -->
            <div class="form-section">
              <h3 class="section-title">Personal Details</h3>
              
              <div class="form-row">
                <div class="form-group">
                  <label for="firstname">First Name<span class="required">*</span></label>
                  <input type="text" id="firstname" v-model="directorsClubData.firstName" required>
                </div>
                <div class="form-group">
                border-radius: 0;
                box-shadow: none;
                  <input type="text" id="lastname" v-model="directorsClubData.lastName" required>
                </div>
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="initial">Initial</label>
                  <input type="text" id="initial" v-model="directorsClubData.initial" maxlength="1">
                </div>
                <div class="form-group">
                  <label for="birthday">Birthday<span class="required">*</span></label>
                  <input type="date" id="birthday" v-model="directorsClubData.birthday" required>
                </div>
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="favcinema">Favourite Cinema</label>
                  <select id="favcinema" v-model="directorsClubData.favouriteCinema">
                    <option value="">None</option>
                    <option value="SM ANGONO">SM ANGONO</option>
                    <option value="SM AURA PREMIER">SM AURA PREMIER</option>
                    <option value="SM MEGAMALL">SM MEGAMALL</option>
                    <option value="SM MALL OF ASIA">SM MALL OF ASIA / S'MAISON</option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="gender">Gender</label>
                  <select id="gender" v-model="directorsClubData.gender">
                    <option value="">Select Gender</option>
                    <option value="Male">Male</option>
                    <option value="Female">Female</option>
                    <option value="Other">Other</option>
                  </select>
                </div>
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="householdsize">Household Size</label>
                  <select id="householdsize" v-model="directorsClubData.householdSize">
                    <option value="">Select Size</option>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4+">4+</option>
                  </select>
                </div>
                <div class="form-group">
                  <label for="maritalstatus">Marital Status</label>
                  <select id="maritalstatus" v-model="directorsClubData.maritalStatus">
                    <option value="">Select Status</option>
                    <option value="Single">Single</option>
                    <option value="Married">Married</option>
                    <option value="Divorced">Divorced</option>
                    <option value="Widowed">Widowed</option>
                  </select>
                </div>
              </div>
            </div>

            <!-- Contact Details Section -->
            <div class="form-section">
              <h3 class="section-title">Contact Details</h3>
              
              <div class="form-group">
                <label for="email">Email<span class="required">*</span></label>
                <input type="email" id="email" v-model="directorsClubData.email" required>
              </div>

              <div class="checkbox-group">
                <label class="checkbox-label">
                  <input type="checkbox" v-model="directorsClubData.emailOptIn">
                  Please send me your newsletter
                </label>
                <label class="checkbox-label">
                  <input type="checkbox" v-model="directorsClubData.allowThirdParty">
                  Allow third parties to send me special offers
                </label>
                <label class="checkbox-label">
                  <input type="checkbox" v-model="directorsClubData.allowDataSharing">
                  Allow my data to be shared with this cinema's market analysis partners
                </label>
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="phone">Phone</label>
                  <input type="tel" id="phone" v-model="directorsClubData.phone">
                </div>
                <div class="form-group">
                  <label for="mobile">Mobile</label>
                  <input type="tel" id="mobile" v-model="directorsClubData.mobile">
                </div>
              </div>

              <div class="form-group">
                <label for="address">Address</label>
                <input type="text" id="address" v-model="directorsClubData.address">
              </div>

              <div class="form-row">
                <div class="form-group">
                  <label for="suburb">Suburb</label>
                  <input type="text" id="suburb" v-model="directorsClubData.suburb">
                </div>
                <div class="form-group">
                  <label for="postcode">Postcode<span class="required">*</span></label>
                  <input type="text" id="postcode" v-model="directorsClubData.postcode" required>
                </div>
              </div>

              <div class="form-group">
                <label for="city">City</label>
                <input type="text" id="city" v-model="directorsClubData.city">
              </div>
            </div>

            <!-- Preferences Section -->
            <div class="form-section">
              <div class="preference-header">
                <h3 class="section-title">My Preferences</h3>
                <button type="button" class="toggle-btn" @click="showPreferences = !showPreferences">
                  {{ showPreferences ? 'Hide' : 'Show' }}
                </button>
              </div>
              
              <div v-if="showPreferences" class="preferences-content">
              <div class="preference-group">
                <label class="preference-label">Preferred Sites</label>
                <div class="checkbox-group-multi">
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Aura Premier" v-model="directorsClubData.preferredSites">
                    SM Aura Premier
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Center Angono" v-model="directorsClubData.preferredSites">
                    SM Center Angono
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Center Bacoor" v-model="directorsClubData.preferredSites">
                    SM Center Bacoor
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Baguio" v-model="directorsClubData.preferredSites">
                    SM City Baguio
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Baliwag" v-model="directorsClubData.preferredSites">
                    SM City Baliwag
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Bataan" v-model="directorsClubData.preferredSites">
                    SM City Bataan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Batangas" v-model="directorsClubData.preferredSites">
                    SM City Batangas
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City BF Paranaque" v-model="directorsClubData.preferredSites">
                    SM City BF Paranaque
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Bicutan" v-model="directorsClubData.preferredSites">
                    SM City Bicutan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Butuan" v-model="directorsClubData.preferredSites">
                    SM City Butuan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Cabanatuan" v-model="directorsClubData.preferredSites">
                    SM City Cabanatuan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Calamba" v-model="directorsClubData.preferredSites">
                    SM City Calamba
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Caloocan" v-model="directorsClubData.preferredSites">
                    SM City Caloocan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Cauayan" v-model="directorsClubData.preferredSites">
                    SM City Cauayan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Cagayan de Oro" v-model="directorsClubData.preferredSites">
                    SM City Cagayan de Oro
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Cebu" v-model="directorsClubData.preferredSites">
                    SM City Cebu
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Clark" v-model="directorsClubData.preferredSites">
                    SM City Clark
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Consolacion" v-model="directorsClubData.preferredSites">
                    SM City Consolacion
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Daet" v-model="directorsClubData.preferredSites">
                    SM City Daet
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Dasmarinas" v-model="directorsClubData.preferredSites">
                    SM City Dasmarinas
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Davao" v-model="directorsClubData.preferredSites">
                    SM City Davao
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City East Ortigas" v-model="directorsClubData.preferredSites">
                    SM City East Ortigas
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Fairview" v-model="directorsClubData.preferredSites">
                    SM City Fairview
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City General Santos" v-model="directorsClubData.preferredSites">
                    SM City General Santos
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Grand Central" v-model="directorsClubData.preferredSites">
                    SM City Grand Central
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Iloilo" v-model="directorsClubData.preferredSites">
                    SM City Iloilo
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City J Mall Cebu" v-model="directorsClubData.preferredSites">
                    SM City J Mall Cebu
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City La Union" v-model="directorsClubData.preferredSites">
                    SM City La Union
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Laoag" v-model="directorsClubData.preferredSites">
                    SM City Laoag
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Legazpi" v-model="directorsClubData.preferredSites">
                    SM City Legazpi
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Lipa" v-model="directorsClubData.preferredSites">
                    SM City Lipa
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Lucena" v-model="directorsClubData.preferredSites">
                    SM City Lucena
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Manila" v-model="directorsClubData.preferredSites">
                    SM City Manila
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Marikina" v-model="directorsClubData.preferredSites">
                    SM City Marikina
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Marilao" v-model="directorsClubData.preferredSites">
                    SM City Marilao
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Masinag" v-model="directorsClubData.preferredSites">
                    SM City Masinag
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Mindpro" v-model="directorsClubData.preferredSites">
                    SM City Mindpro
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Molino" v-model="directorsClubData.preferredSites">
                    SM City Molino
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Naga" v-model="directorsClubData.preferredSites">
                    SM City Naga
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City North Edsa" v-model="directorsClubData.preferredSites">
                    SM City North Edsa
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Novaliches" v-model="directorsClubData.preferredSites">
                    SM City Novaliches
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Olongapo Central" v-model="directorsClubData.preferredSites">
                    SM City Olongapo Central
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Olongapo Downtown" v-model="directorsClubData.preferredSites">
                    SM City Olongapo Downtown
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Pampanga" v-model="directorsClubData.preferredSites">
                    SM City Pampanga
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Puerto Princesa" v-model="directorsClubData.preferredSites">
                    SM City Puerto Princesa
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Rosales" v-model="directorsClubData.preferredSites">
                    SM City Rosales
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Rosario" v-model="directorsClubData.preferredSites">
                    SM City Rosario
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Roxas" v-model="directorsClubData.preferredSites">
                    SM City Roxas
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City San Jose del Monte" v-model="directorsClubData.preferredSites">
                    SM City San Jose del Monte
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City San Lazaro" v-model="directorsClubData.preferredSites">
                    SM City San Lazaro
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City San Mateo" v-model="directorsClubData.preferredSites">
                    SM City San Mateo
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City San Pablo" v-model="directorsClubData.preferredSites">
                    SM City San Pablo
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Santa Rosa" v-model="directorsClubData.preferredSites">
                    SM City Santa Rosa
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Sorsogon" v-model="directorsClubData.preferredSites">
                    SM City Sorsogon
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Sta. Mesa" v-model="directorsClubData.preferredSites">
                    SM City Sta. Mesa
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Sto Tomas" v-model="directorsClubData.preferredSites">
                    SM City Sto Tomas
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Sucat" v-model="directorsClubData.preferredSites">
                    SM City Sucat
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Tanza" v-model="directorsClubData.preferredSites">
                    SM City Tanza
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Tarlac" v-model="directorsClubData.preferredSites">
                    SM City Tarlac
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Taytay" v-model="directorsClubData.preferredSites">
                    SM City Taytay
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Telabastagan" v-model="directorsClubData.preferredSites">
                    SM City Telabastagan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Trece Martires" v-model="directorsClubData.preferredSites">
                    SM City Trece Martires
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Tuguegarao" v-model="directorsClubData.preferredSites">
                    SM City Tuguegarao
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Urdaneta Central" v-model="directorsClubData.preferredSites">
                    SM City Urdaneta Central
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM City Valenzuela" v-model="directorsClubData.preferredSites">
                    SM City Valenzuela
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM CDO Downtown Premier" v-model="directorsClubData.preferredSites">
                    SM CDO Downtown Premier
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Center Ormoc" v-model="directorsClubData.preferredSites">
                    SM Center Ormoc
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Center Pulilan" v-model="directorsClubData.preferredSites">
                    SM Center Pulilan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Center Sangandaan" v-model="directorsClubData.preferredSites">
                    SM Center Sangandaan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Lanang Premier" v-model="directorsClubData.preferredSites">
                    SM Lanang Premier
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Mall of Asia" v-model="directorsClubData.preferredSites">
                    SM Mall of Asia
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="S Maison" v-model="directorsClubData.preferredSites">
                    S Maison
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Megacenter Cabanatuan" v-model="directorsClubData.preferredSites">
                    SM Megacenter Cabanatuan
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Megamall" v-model="directorsClubData.preferredSites">
                    SM Megamall
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM San Fernando" v-model="directorsClubData.preferredSites">
                    SM San Fernando
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Seaside City Cebu" v-model="directorsClubData.preferredSites">
                    SM Seaside City Cebu
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="SM Southmall" v-model="directorsClubData.preferredSites">
                    SM Southmall
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="The Podium Mall" v-model="directorsClubData.preferredSites">
                    The Podium Mall
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Light Residences" v-model="directorsClubData.preferredSites">
                    Light Residences
                  </label>
                </div>
              </div>

              <div class="preference-group">
                <label class="preference-label">Preferred Genres</label>
                <div class="checkbox-group-multi">
                  <label class="checkbox-label">
                    <input type="checkbox" value="Action" v-model="directorsClubData.preferredGenres">
                    Action
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Comedy" v-model="directorsClubData.preferredGenres">
                    Comedy
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Drama" v-model="directorsClubData.preferredGenres">
                    Drama
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Horror" v-model="directorsClubData.preferredGenres">
                    Horror
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Romance" v-model="directorsClubData.preferredGenres">
                    Romance
                  </label>
                  <label class="checkbox-label">
                    <input type="checkbox" value="Sci-Fi" v-model="directorsClubData.preferredGenres">
                    Sci-Fi
                  </label>
                </div>
              </div>
            </div>
        </div>

            <div class="form-section">
              <h3 class="section-title">Create Login</h3>
              
              <div class="form-group">
                <label for="password">Password<span class="required">*</span></label>
                <input type="password" id="password" v-model="directorsClubData.password" required>
              </div>

              <div class="form-group">
                <label for="confirmpassword">Confirm Password<span class="required">*</span></label>
                <input type="password" id="confirmpassword" v-model="directorsClubData.confirmPassword" required>
              </div>

              <label class="checkbox-label terms-checkbox">
                <input type="checkbox" v-model="directorsClubData.agreeToTerms" required>
                I Agree to the Terms and Conditions
              </label>
            </div>

            <button type="submit" class="submit-btn">Pay Now</button>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'
import PickTicketsModal from './PickTicketsModal.vue'

const emit = defineEmits(['navigate'])
const showMembershipDetails = ref(false)
const showDirectorsClubForm = ref(false)
const showPreferences = ref(true)
const loginData = ref({
  email: '',
  password: ''
})

const directorsClubData = ref({
  firstName: '',
  lastName: '',
  initial: '',
  birthday: '',
  favouriteCinema: '',
  gender: '',
  householdSize: '',
  maritalStatus: '',
  email: '',
  emailOptIn: false,
  allowThirdParty: false,
  allowDataSharing: false,
  phone: '',
  mobile: '',
  address: '',
  suburb: '',
  postcode: '',
  city: '',
  preferredSites: [],
  preferredGenres: [],
  password: '',
  confirmPassword: '',
  agreeToTerms: false
})

const cinemaBanners = [
  new URL('../assets/Banners/Cinema_Banners/banner_1.jpeg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_2.jpg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_3.jpg', import.meta.url).href,
  new URL('../assets/Banners/Cinema_Banners/banner_4.jpeg', import.meta.url).href
]
const loyaltyBannerImage = new URL('../assets/Banners/Loyalty_Banners/Pic_1.jpg', import.meta.url).href
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

const openSignUpForm = () => {
  showMembershipDetails.value = true
}

const goBack = () => {
  showMembershipDetails.value = false
}

const openDirectorsClubForm = () => {
  showDirectorsClubForm.value = true
}

const goBackToMembership = () => {
  showDirectorsClubForm.value = false
}

const handleDirectorsClubSignUp = () => {
  if (directorsClubData.value.password !== directorsClubData.value.confirmPassword) {
    alert('Passwords do not match!')
    return
  }
  console.log('Director\'s Club Sign Up:', directorsClubData.value)
  alert('Sign up submitted successfully!')
  goBackToMembership()
}

const handleLogin = () => {
  // Handle login logic here
  console.log('Login attempt:', loginData.value)
  alert('Please log in at www.smcinema.com')
}
</script>

<style scoped>
.loyalty-section {
  padding: 3rem 0;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Pick Tickets Section */
.pick-tickets {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  background-color: rgb(249, 44, 29);
  padding: 2rem;
  border-radius: 8px;
  margin-bottom: 3rem;
  align-items: center;
}

.pick-tickets-content h2 {
  font-size: 1.5rem;
  font-weight: 700;
  color: white;
  margin-bottom: 0.5rem;
}

.pick-tickets-content p {
  color: rgba(255, 255, 255, 0.9);
  margin-bottom: 1.5rem;
  line-height: 1.6;
}

.pick-buttons {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.pick-btn {
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

.pick-btn:hover {
  background-color: white;
  color: rgb(249, 44, 29);
}

.imax-banner {
  position: relative;
  overflow: hidden;
  padding: 0;
  border-radius: 0;
  background: transparent;
}

.imax-content {
  position: absolute;
  inset: 1.5rem;
  z-index: 1;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  color: white;
  text-shadow: 0 2px 10px rgba(0, 0, 0, 0.6);
}

.imax-logo {
  color: #00a8ff;
  font-weight: 700;
  font-size: 1.3rem;
}

.imax-content h3 {
  color: white;
  font-size: 1.5rem;
  font-weight: 700;
  line-height: 1.2;
}

.imax-image {
  width: 120%;
  height: auto;
  object-fit: cover;
  display: block;
  border-radius: 0;
  margin-left: -10%;
}

/* Loyalty Banner Section */
.loyalty-banner-section {
  margin-bottom: 3rem;
}

.loyalty-banner {
  position: relative;
  border-radius: 10px;
  overflow: hidden;
  box-shadow: none;
}

.loyalty-banner-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.loyalty-banner-content {
  position: absolute;
  inset: 1.5rem;
  color: white;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  text-shadow: 0 2px 15px rgba(0, 0, 0, 0.8);
}

.loyalty-banner-content .badge {
  font-size: 0.85rem;
  letter-spacing: 0.3em;
  color: rgba(255, 255, 255, 0.85);
  font-weight: 600;
}

.loyalty-banner-content h2 {
  font-size: 2.1rem;
  font-weight: 700;
  margin: 0;
}

.loyalty-banner-content span {
  color: #ffd700;
}

.loyalty-banner-content .subtext {
  font-size: 1rem;
  max-width: 420px;
  margin: 0;
}

.loyalty-learn-btn {
  align-self: flex-start;
  padding: 0.75rem 2rem;
  background-color: rgba(0, 0, 0, 0.7);
  border: 1px solid rgba(255, 255, 255, 0.6);
  border-radius: 999px;
  color: white;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
}

.loyalty-learn-btn:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

/* Action Section */
.action-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  margin-bottom: 3rem;
  align-items: start;
}

.action-left h3 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 1rem;
}

.action-left h4 {
  margin-bottom: 1rem;
}

.find-more {
  display: inline-block;
  color: #0052cc;
  text-decoration: underline;
  font-weight: 600;
  margin-bottom: 1rem;
}

.signup-btn {
  display: block;
  padding: 0.75rem 2rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.signup-btn:hover {
  background-color: #D62828;
}

.club-login {
  background-color: white;
  border: 2px solid #333;
  padding: 2rem;
  border-radius: 4px;
}

.club-login h3 {
  font-size: 1.3rem;
  font-weight: 700;
  color: #333;
  margin: 0 0 1.5rem 0;
  text-align: center;
}

.login-form {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.form-input {
  padding: 0.75rem 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  font-family: inherit;
  transition: border-color 0.3s ease;
  background-color: white;
}

.form-input::placeholder {
  color: #aaa;
  font-style: italic;
}

.form-input:focus {
  outline: none;
  border-color: rgb(249, 44, 29);
  box-shadow: 0 0 0 3px rgba(249, 44, 29, 0.1);
}

.form-links {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}

.forgot-link {
  color: rgb(249, 44, 29);
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 600;
}

.forgot-link:hover {
  text-decoration: underline;
}

.sign-in-btn {
  padding: 0.75rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: background-color 0.3s ease;
  white-space: nowrap;
}

.sign-in-btn:hover {
  background-color: #D62828;
}

/* Membership Details Section */
.membership-details {
  margin-bottom: 3rem;
}

.back-btn {
  padding: 0.5rem 1rem;
  background-color: transparent;
  border: 1px solid #333;
  border-radius: 4px;
  font-weight: 600;
  font-size: 0.95rem;
  cursor: pointer;
  color: #333;
  margin-bottom: 2rem;
  transition: all 0.3s ease;
}

.back-btn:hover {
  background-color: #f0f0f0;
  border-color: #000;
}

.membership-content {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
}

.membership-left h2 {
  font-size: 1.8rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.5rem;
  border-bottom: 2px solid #333;
  padding-bottom: 1rem;
}

.membership-description {
  color: #666;
  font-size: 0.95rem;
  margin-bottom: 1.5rem;
}

.membership-description-box {
  background-color: #f9f9f9;
  padding: 1.5rem;
  border-left: 4px solid rgb(249, 44, 29);
  border-radius: 4px;
}

.membership-description-box p {
  color: #333;
  font-size: 0.95rem;
  margin-bottom: 1rem;
}

.membership-description-box h4 {
  font-size: 1rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.75rem;
}

.benefits-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.benefits-list li {
  color: #333;
  font-size: 0.95rem;
  margin-bottom: 0.5rem;
  padding-left: 1.5rem;
  position: relative;
}

.benefits-list li:before {
  content: '✓';
  position: absolute;
  left: 0;
  color: rgb(249, 44, 29);
  font-weight: 700;
}

.offer-card {
  background-color: #000;
  color: white;
  padding: 2rem;
  border-radius: 4px;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

.offer-header {
  font-size: 0.8rem;
  font-weight: 700;
  color: white;
  letter-spacing: 1px;
  margin-bottom: 0.5rem;
  text-align: center;
}

.offer-card h3 {
  font-size: 1.8rem;
  font-weight: 700;
  color: white;
  line-height: 1.2;
  margin-bottom: 1rem;
  text-align: center;
}

.offer-description {
  color: rgba(255, 255, 255, 0.9);
  font-size: 0.85rem;
  margin-bottom: 1rem;
  line-height: 1.5;
  text-align: center;
}

.offer-price {
  font-size: 1.8rem;
  font-weight: 700;
  color: white;
  text-align: center;
  margin-bottom: 1rem;
}

.signup-now-btn {
  padding: 0.75rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  margin-bottom: 1rem;
  transition: background-color 0.3s ease;
}

.signup-now-btn:hover {
  background-color: #D62828;
}

.offer-image {
  width: 100%;
  height: auto;
  border-radius: 4px;
  max-height: 200px;
  object-fit: cover;
}

/* Sign Up Form Section */
.signup-form-section {
  margin-bottom: 3rem;
}

.form-wrapper {
  background-color: white;
  padding: 2.5rem;
  border-radius: 8px;
  border: 1px solid #ddd;
}

.form-wrapper h2 {
  font-size: 1.5rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 2rem;
  border-bottom: 2px solid #ddd;
  padding-bottom: 1rem;
}

.membership-form {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.form-section {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid #eee;
}

.form-section:last-of-type {
  border-bottom: none;
  padding-bottom: 0;
}

.section-title {
  font-size: 1.1rem;
  font-weight: 700;
  color: #333;
  margin: 0 0 1rem 0;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.form-group {
  display: flex;
  flex-direction: column;
}

.form-group label {
  font-weight: 600;
  color: #333;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

.required {
  color: rgb(249, 44, 29);
}

.form-group input,
.form-group select {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.95rem;
  font-family: inherit;
  transition: border-color 0.3s ease;
  background-color: white;
}

.form-group input:focus,
.form-group select:focus {
  outline: none;
  border-color: rgb(249, 44, 29);
  box-shadow: 0 0 0 3px rgba(249, 44, 29, 0.1);
}

.checkbox-group {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin: 0.5rem 0;
}

.checkbox-group-multi {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.75rem;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #333;
  font-weight: 500;
  cursor: pointer;
  font-size: 0.9rem;
}

.checkbox-label input {
  width: 18px;
  height: 18px;
  cursor: pointer;
  margin: 0;
  flex-shrink: 0;
}

.terms-checkbox {
  margin-top: 1rem;
  font-weight: 600;
}

.preference-group {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.preference-label {
  font-weight: 700;
  color: #333;
  font-size: 0.95rem;
}

.preference-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
}

.toggle-btn {
  padding: 0.4rem 0.8rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  font-size: 0.85rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-btn:hover {
  background-color: #D62828;
}

.preferences-content {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.submit-btn {
  padding: 0.85rem 2rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
  align-self: flex-start;
  margin-top: 1rem;
}

.submit-btn:hover {
  background-color: #D62828;
}

/* Responsive Design */
@media (max-width: 768px) {
  .pick-tickets {
    grid-template-columns: 1fr;
    padding: 1.5rem;
  }

  .pick-buttons {
    grid-template-columns: 1fr 1fr;
  }

  .loyalty-card-section {
    padding: 1.5rem;
  }

  .card-content {
    grid-template-columns: 1fr;
    gap: 1.5rem;
  }

  .card-image {
    order: -1;
  }

  .card-text h2 {
    font-size: 1.5rem;
  }

  .action-section {
    grid-template-columns: 1fr;
    gap: 2rem;
  }
}
</style>
