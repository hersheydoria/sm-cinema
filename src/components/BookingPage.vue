<template>
  <section class="booking-section">
    <div class="container">
      <!-- Booking Steps Progress -->
      <div class="booking-steps">
        <div v-for="(stepInfo, idx) in steps" :key="idx" class="step" :class="{ active: currentStep === idx + 1, completed: currentStep > idx + 1 }">
          <div class="step-circle">{{ idx + 1 }}</div>
          <div class="step-label">{{ stepInfo }}</div>
        </div>
      </div>

      <div class="booking-wrapper">
        <!-- Movie Details Header (Fixed across all steps) -->
        <div v-if="currentStep < 5" class="movie-details-header">
          <img :src="selectedMovie?.poster" :alt="selectedMovie?.title" class="movie-poster-small" />
          <div class="header-info">
            <h3>{{ selectedMovie?.title }}</h3>
            <p>
              <span v-if="selectedShowType">{{ selectedShowType }} • </span>
              {{ selectedDate }} • {{ selectedTime }} • {{ selectedCinema?.name }}
            </p>
          </div>
        </div>

        <!-- Step Content -->
        <div class="booking-content" :class="{ 'full-width': currentStep === 5 }">
          <!-- Step 1: Select Tickets -->
          <div v-if="currentStep === 1" class="step-content">
            <div class="step-title">
              <h2>SELECT TICKETS</h2>
              <p>Choose the number and type of tickets you wish to buy. Maximum 8 tickets per transaction.</p>
            </div>

            <!-- Ticket Type Tabs -->
            <div class="ticket-tabs">
              <button 
                class="tab-btn" 
                :class="{ active: ticketTabActive === 'standard' }"
                @click="ticketTabActive = 'standard'"
              >
                STANDARD
              </button>
              <button 
                class="tab-btn" 
                :class="{ active: ticketTabActive === 'voucher' }"
                @click="ticketTabActive = 'voucher'"
              >
                VOUCHER
              </button>
            </div>

            <!-- Standard Tab Content -->
            <div v-if="ticketTabActive === 'standard'" class="tab-content">
              <div class="tickets-selection">
                <div class="ticket-row">
                  <div class="ticket-info">
                    <span class="ticket-name">2D Regular</span>
                    <span class="ticket-price">₱330.00</span>
                  </div>
                  <div class="qty-control">
                    <button @click="updateTicketQty('2d', -1)" class="qty-btn">−</button>
                    <input v-model.number="ticketQty['2d']" type="number" readonly />
                    <button @click="updateTicketQty('2d', 1)" class="qty-btn">+</button>
                  </div>
                </div>

                <div class="ticket-row">
                  <div class="ticket-info">
                    <span class="ticket-name">3D Regular</span>
                    <span class="ticket-price">₱380.00</span>
                  </div>
                  <div class="qty-control">
                    <button @click="updateTicketQty('3d', -1)" class="qty-btn">−</button>
                    <input v-model.number="ticketQty['3d']" type="number" readonly />
                    <button @click="updateTicketQty('3d', 1)" class="qty-btn">+</button>
                  </div>
                </div>

                <div class="ticket-row">
                  <div class="ticket-info">
                    <span class="ticket-name">IMAX</span>
                    <span class="ticket-price">₱450.00</span>
                  </div>
                  <div class="qty-control">
                    <button @click="updateTicketQty('imax', -1)" class="qty-btn">−</button>
                    <input v-model.number="ticketQty['imax']" type="number" readonly />
                    <button @click="updateTicketQty('imax', 1)" class="qty-btn">+</button>
                  </div>
                </div>
              </div>
            </div>

            <!-- Voucher Tab Content -->
            <div v-else-if="ticketTabActive === 'voucher'" class="tab-content">
              <div class="voucher-container">
                <div class="voucher-input-section">
                  <label for="voucher-code">Enter a ticket voucher code</label>
                  <div class="voucher-input-group">
                    <input 
                      id="voucher-code"
                      v-model="voucherCode" 
                      type="text" 
                      placeholder="Enter voucher code"
                      class="voucher-input"
                      @keyup.enter="addVoucherCode"
                    />
                    <button class="add-voucher-btn" @click="addVoucherCode">Add to Order</button>
                  </div>
                </div>

                <div v-if="appliedVouchers.length > 0" class="applied-vouchers">
                  <h4>Applied Vouchers</h4>
                  <div class="voucher-list">
                    <div v-for="(voucher, idx) in appliedVouchers" :key="idx" class="voucher-item">
                      <div class="voucher-details">
                        <span class="voucher-code">{{ voucher.code }}</span>
                        <span class="voucher-type">{{ voucher.type }}</span>
                      </div>
                      <button class="remove-voucher" @click="removeVoucher(idx)">✕</button>
                    </div>
                  </div>
                </div>

                <div v-else class="no-vouchers">
                  <p>No vouchers applied yet</p>
                </div>
              </div>
            </div>

            <div class="sm-club-section">
              <h4>SM Cinema Club Member?</h4>
              <p>Login to earn more points on your purchase</p>
              <button class="login-btn">Sign In / Sign Up</button>
            </div>
          </div>

          <!-- Step 2: Select Seats -->
          <div v-else-if="currentStep === 2" class="step-content">
            <div class="step-title">
              <h2>SELECT SEATS</h2>
              <p>Choose your preferred seats in the cinema.</p>
            </div>

            <div class="seats-selection">
              <div class="screen">SCREEN</div>
              <div class="seat-grid">
                <div v-for="row in 8" :key="row" class="seat-row">
                  <div v-for="col in 12" :key="`${row}-${col}`" class="seat" :class="{ selected: selectedSeats.includes(`${row}-${col}`), occupied: Math.random() > 0.7 }" @click="toggleSeat(`${row}-${col}`)">
                    {{ String.fromCharCode(64 + row) }}{{ col }}
                  </div>
                </div>
              </div>
              <div class="seat-legend">
                <div class="legend-item"><span class="seat-dot available"></span> Available</div>
                <div class="legend-item"><span class="seat-dot selected"></span> Selected</div>
                <div class="legend-item"><span class="seat-dot occupied"></span> Occupied</div>
              </div>
            </div>

            <p class="seat-notice">*Please ensure that you are selecting seats for the correct branch of your choice</p>
          </div>

          <!-- Step 3: Get Your Concessions -->
          <div v-else-if="currentStep === 3" class="step-content">
            <div class="step-title">
              <h2>GET YOUR CONCESSIONS</h2>
              <p>Add snacks and drinks to your order (optional)</p>
            </div>

            <div class="concessions-grid">
              <div v-for="item in concessionItems" :key="item.id" class="concession-card">
                <div class="concession-image">{{ item.emoji }}</div>
                <h4>{{ item.name }}</h4>
                <p class="concession-price">₱{{ item.price }}</p>
                <div class="concession-qty">
                  <button @click="updateConcessionQty(item.id, -1)" class="qty-btn">−</button>
                  <span class="qty-display">{{ concessionQty[item.id] }}</span>
                  <button @click="updateConcessionQty(item.id, 1)" class="qty-btn">+</button>
                </div>
              </div>
            </div>
          </div>

          <!-- Step 4: Confirm -->
          <div v-else-if="currentStep === 4" class="step-content confirm-step">
            <div class="step-title">
              <h2>CONFIRM YOUR BOOKING</h2>
              <p>Review and confirm your order details before payment</p>
            </div>

            <div class="confirm-wrapper">
              <!-- Left Content -->
              <div class="confirm-content">
                <!-- Personal Details Section -->
                <div class="form-section">
                  <h3>PERSONAL DETAILS</h3>
                  <div class="form-group">
                    <label for="name">Name*</label>
                    <input id="name" v-model="personalDetails.name" type="text" placeholder="Enter your full name" class="form-input" />
                  </div>
                  <div class="form-group">
                    <label for="email">Email*</label>
                    <input id="email" v-model="personalDetails.email" type="email" placeholder="Enter your email address" class="form-input" />
                  </div>
                  <div class="form-group">
                    <label for="phone">Phone (optional)</label>
                    <input id="phone" v-model="personalDetails.phone" type="tel" placeholder="Enter your phone number" class="form-input" />
                  </div>
                  <div class="form-group">
                    <label for="comments">Pickup Comments (optional)</label>
                    <textarea id="comments" v-model="personalDetails.comments" placeholder="Add any special requests or comments" class="form-input textarea"></textarea>
                  </div>
                </div>

                <!-- Payment Method Section -->
                <div class="form-section">
                  <h3>PAYMENT METHOD</h3>
                  <div class="payment-options">
                    <label class="payment-option">
                      <input v-model="selectedPaymentMethod" type="radio" value="credit" />
                      <span class="payment-label">CREDIT/DEBIT</span>
                    </label>
                  </div>

                  <div class="payment-note">
                    <p class="note-text">By providing your personal data and clicking Submit, you <span class="highlight">Consent</span> and that you have read our <span class="highlight">Data Privacy Policy</span></p>
                    <label class="checkbox-label">
                      <input v-model="agreeTerms" type="checkbox" />
                      <span>I have read and understood the <span class="highlight">Terms and Conditions</span></span>
                    </label>
                  </div>

                  <p class="important-note">*Always check your order summary before proceeding to payment.</p>
                </div>

                <!-- Action Buttons -->
                <div class="confirm-actions">
                  <button class="btn-cancel" @click="cancelBooking">Cancel Order</button>
                  <button class="btn-next" @click="proceedToPayment" :disabled="!canCompleteBooking">Next</button>
                </div>
              </div>

              <!-- Right Sidebar - Order Summary -->
              <div class="order-summary-sidebar">
                <div class="summary-box">
                  <div class="time-remaining">
                    TIME REMAINING: <span class="timer">29:27</span>
                  </div>

                  <!-- Your Basket -->
                  <div class="basket-section">
                    <h4>YOUR BASKET</h4>
                    
                    <div class="gift-card-section">
                      <input v-model="giftCardCode" type="text" placeholder="Gift Card No." class="gift-card-input" />
                      <label class="balance-label">
                        <input v-model="useGiftCardBalance" type="checkbox" />
                        <span>Use available balance</span>
                      </label>
                      <button class="btn-apply">Apply</button>
                    </div>

                    <!-- Movie Info -->
                    <div class="basket-item movie-item">
                      <h5>{{ selectedMovie?.title }}</h5>
                      <p class="movie-details">Showing at Sat 15 Nov 2:00PM SM City Butuan, Cinema 2</p>
                    </div>

                    <!-- Items Table -->
                    <div class="items-table">
                      <div class="table-header">
                        <span class="col-item">Items</span>
                        <span class="col-cost">Cost</span>
                        <span class="col-qty">Qty</span>
                        <span class="col-subtotal">Subtotal</span>
                      </div>
                      <div v-if="ticketQty['2d'] > 0" class="table-row">
                        <span class="col-item">2D REG*</span>
                        <span class="col-cost">330.00</span>
                        <span class="col-qty">{{ ticketQty['2d'] }}</span>
                        <span class="col-subtotal">{{ (ticketQty['2d'] * 330).toFixed(2) }}</span>
                      </div>
                      <div v-if="ticketQty['3d'] > 0" class="table-row">
                        <span class="col-item">3D REG*</span>
                        <span class="col-cost">380.00</span>
                        <span class="col-qty">{{ ticketQty['3d'] }}</span>
                        <span class="col-subtotal">{{ (ticketQty['3d'] * 380).toFixed(2) }}</span>
                      </div>
                      <div v-if="ticketQty['imax'] > 0" class="table-row">
                        <span class="col-item">IMAX</span>
                        <span class="col-cost">450.00</span>
                        <span class="col-qty">{{ ticketQty['imax'] }}</span>
                        <span class="col-subtotal">{{ (ticketQty['imax'] * 450).toFixed(2) }}</span>
                      </div>
                    </div>

                    <!-- Totals -->
                    <div class="totals-section">
                      <div class="total-row">
                        <span>Booking Fee</span>
                        <span>₱50.00</span>
                      </div>
                      <div class="total-row grand-total">
                        <span>Total</span>
                        <span>₱{{ (getTicketTotal() + 50).toFixed(2) }}</span>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Step 5: Booking Successful -->
          <div v-else-if="currentStep === 5" class="step-content success-step">
            <div class="success-container">
              <div class="success-icon">✓</div>
              <h2>Booking Successful!</h2>
              <p class="success-message">Your booking has been confirmed. A confirmation email has been sent to your registered email address.</p>

              <div class="booking-confirmation-card">
                <div class="confirmation-header">
                  <h3>{{ selectedMovie?.title }}</h3>
                  <div class="confirmation-details">
                    <p><strong>Date & Time:</strong> {{ selectedDate }} at {{ selectedTime }}</p>
                    <p><strong>Cinema:</strong> {{ selectedCinema?.name }} - Cinema 2</p>
                    <p><strong>Seats:</strong> {{ selectedSeats.join(', ') }}</p>
                  </div>
                </div>

                <div class="confirmation-body">
                  <h4>Order Summary</h4>
                  <div class="summary-table">
                    <div v-if="ticketQty['2d'] > 0" class="summary-row">
                      <span>{{ ticketQty['2d'] }}x 2D Regular</span>
                      <span>₱{{ (ticketQty['2d'] * 330).toFixed(2) }}</span>
                    </div>
                    <div v-if="ticketQty['3d'] > 0" class="summary-row">
                      <span>{{ ticketQty['3d'] }}x 3D Regular</span>
                      <span>₱{{ (ticketQty['3d'] * 380).toFixed(2) }}</span>
                    </div>
                    <div v-if="ticketQty['imax'] > 0" class="summary-row">
                      <span>{{ ticketQty['imax'] }}x IMAX</span>
                      <span>₱{{ (ticketQty['imax'] * 450).toFixed(2) }}</span>
                    </div>
                    <div class="summary-row total">
                      <span>Total Amount</span>
                      <span>₱{{ (getTicketTotal() + 50).toFixed(2) }}</span>
                    </div>
                  </div>
                </div>

                <div class="confirmation-footer">
                  <p><strong>Reference Number:</strong> BK{{ Math.random().toString(36).substr(2, 9).toUpperCase() }}</p>
                  <p>Please present this reference number at the cinema counter when collecting your tickets.</p>
                </div>
              </div>

              <div class="success-actions">
                <button class="btn-download" @click="downloadReceipt">Download Receipt</button>
                <button class="btn-home" @click="goHome">Back to Home</button>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Sidebar - Summary -->
        <div v-if="currentStep < 5" class="booking-sidebar">
          <div class="summary-header">
            <h3>ORDER SUMMARY</h3>
          </div>

          <div class="summary-content">
            <div class="summary-section">
              <div class="summary-label">Tickets</div>
              <div class="summary-amount">{{ getTotalTickets() }} tickets</div>
            </div>

            <div class="summary-section">
              <div class="summary-label">Seats</div>
              <div class="summary-amount">{{ selectedSeats.length }} seats</div>
            </div>

            <div v-if="getTotalConcessions() > 0" class="summary-section">
              <div class="summary-label">Concessions</div>
              <div class="summary-amount">{{ getTotalConcessions() }} items</div>
            </div>

            <div class="summary-divider"></div>

            <div class="price-breakdown">
              <div class="breakdown-item">
                <span>Ticket Cost</span>
                <span>₱{{ getTicketTotal().toFixed(2) }}</span>
              </div>
              <div v-if="getConcessionTotal() > 0" class="breakdown-item">
                <span>Concession Cost</span>
                <span>₱{{ getConcessionTotal().toFixed(2) }}</span>
              </div>
              <div class="breakdown-item total">
                <span>Total Amount</span>
                <span>₱{{ (getTicketTotal() + getConcessionTotal()).toFixed(2) }}</span>
              </div>
            </div>

            <p class="booking-note">(Excl. booking fee)</p>
          </div>

          <div class="sidebar-actions">
            <button v-if="currentStep > 1 && currentStep < 5" class="action-btn back-btn" @click="previousStep">← Back</button>
            <button v-if="currentStep < 4" class="action-btn next-btn" @click="nextStep" :disabled="!canProceedToNext">Next →</button>
            <button v-else-if="currentStep === 4" class="action-btn proceed-btn" @click="proceedToPayment" :disabled="!canCompleteBooking">Next</button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, inject } from 'vue'

const currentPage = inject('currentPage', { value: 'home' })
const bookingData = inject('bookingData', null)

// Current step tracking
const currentStep = ref(1)
const steps = ['Select Tickets', 'Select Seats', 'Get Your Concessions', 'Confirm', 'Booking Successful']

// Ticket tab selection
const ticketTabActive = ref('standard')

// Movie details - use computed to directly access bookingData
const defaultMovie = {
  title: 'Meet, Greet & Bye',
  poster: 'https://images.justwatch.com/poster/307617/s718/godzilla-x-kong-the-new-empire.jpg'
}
const defaultDate = 'Sat 15 Nov'
const defaultTime = '2:00 PM'
const defaultCinema = { name: 'SM City Butuan' }

// Use computed properties to directly read from bookingData
const selectedMovie = computed(() => bookingData?.value?.movie || defaultMovie)
const selectedCinema = computed(() => bookingData?.value?.cinema || defaultCinema)
const selectedDate = computed(() => bookingData?.value?.date || defaultDate)
const selectedTime = computed(() => bookingData?.value?.time || defaultTime)
const selectedShowType = computed(() => bookingData?.value?.showType || null)

// Ticket quantities
const ticketQty = ref({
  '2d': 0,
  '3d': 0,
  imax: 0,
  voucher: 0
})

// Voucher management
const voucherCode = ref('')
const appliedVouchers = ref([])

// Personal details form
const personalDetails = ref({
  name: '',
  email: '',
  phone: '',
  comments: ''
})

// Payment and consent
const selectedPaymentMethod = ref('credit')
const agreeTerms = ref(false)
const giftCardCode = ref('')
const useGiftCardBalance = ref(false)

// Sample voucher codes (for demo purposes)
const validVoucherCodes = {
  'SUMMER20': { type: 'Summer Promo 20% OFF', discount: 0.20 },
  'BUNDLE10': { type: 'Bundle Deal 10% OFF', discount: 0.10 },
  'VIP50': { type: 'VIP Early Bird 50% OFF', discount: 0.50 },
  'STUDENTS15': { type: 'Student Discount 15% OFF', discount: 0.15 }
}

// Selected seats
const selectedSeats = ref([])

// Concession items and quantities
const concessionItems = ref([
  { id: 1, name: 'Popcorn Small', price: 150, emoji: '🍿' },
  { id: 2, name: 'Popcorn Large', price: 200, emoji: '🍿' },
  { id: 3, name: 'Coca Cola Small', price: 80, emoji: '🥤' },
  { id: 4, name: 'Coca Cola Large', price: 120, emoji: '🥤' },
  { id: 5, name: 'Nachos', price: 180, emoji: '🧀' },
  { id: 6, name: 'Hot Dog', price: 120, emoji: '🌭' }
])

const concessionQty = ref({
  1: 0,
  2: 0,
  3: 0,
  4: 0,
  5: 0,
  6: 0
})

// Update ticket quantity
const updateTicketQty = (type, change) => {
  const totalTickets = Object.values(ticketQty.value).reduce((a, b) => a + b, 0)
  const newQty = (ticketQty.value[type] || 0) + change
  if (newQty >= 0 && totalTickets + change <= 8 && newQty <= 8) {
    ticketQty.value[type] = newQty
  }
}

// Toggle seat selection
const toggleSeat = (seatId) => {
  const index = selectedSeats.value.indexOf(seatId)
  if (index > -1) {
    selectedSeats.value.splice(index, 1)
  } else {
    if (getTotalTickets() > selectedSeats.value.length) {
      selectedSeats.value.push(seatId)
    }
  }
}

// Add voucher code
const addVoucherCode = () => {
  const code = voucherCode.value.toUpperCase().trim()
  
  if (!code) {
    alert('Please enter a voucher code')
    return
  }

  if (!validVoucherCodes[code]) {
    alert('Invalid voucher code. Try: SUMMER20, BUNDLE10, VIP50, or STUDENTS15')
    return
  }

  // Check if code is already applied
  if (appliedVouchers.value.some(v => v.code === code)) {
    alert('This voucher code is already applied')
    return
  }

  const voucherInfo = validVoucherCodes[code]
  appliedVouchers.value.push({
    code: code,
    type: voucherInfo.type,
    discount: voucherInfo.discount
  })
  
  voucherCode.value = ''
}

// Remove voucher code
const removeVoucher = (index) => {
  appliedVouchers.value.splice(index, 1)
}

// Update concession quantity
const updateConcessionQty = (itemId, change) => {
  const newQty = (concessionQty.value[itemId] || 0) + change
  if (newQty >= 0) {
    concessionQty.value[itemId] = newQty
  }
}

// Get total tickets
const getTotalTickets = () => {
  return Object.values(ticketQty.value).reduce((a, b) => a + b, 0)
}

// Get total concessions
const getTotalConcessions = () => {
  return Object.values(concessionQty.value).reduce((a, b) => a + b, 0)
}

// Calculate ticket total cost
const getTicketTotal = () => {
  return (
    (ticketQty.value['2d'] || 0) * 330 +
    (ticketQty.value['3d'] || 0) * 380 +
    (ticketQty.value['imax'] || 0) * 450 +
    (ticketQty.value['voucher'] || 0) * 330
  )
}

// Calculate concession total cost
const getConcessionTotal = () => {
  let total = 0
  concessionItems.value.forEach(item => {
    total += (concessionQty.value[item.id] || 0) * item.price
  })
  return total
}

// Check if can proceed to next step
const canProceedToNext = computed(() => {
  if (currentStep.value === 1) {
    return getTotalTickets() > 0
  } else if (currentStep.value === 2) {
    return selectedSeats.value.length === getTotalTickets()
  } else if (currentStep.value === 3) {
    return true
  } else if (currentStep.value === 4) {
    return personalDetails.value.name && personalDetails.value.email && agreeTerms.value
  }
  return false
})

const canCompleteBooking = computed(() => {
  return personalDetails.value.name && personalDetails.value.email && agreeTerms.value
})

// Navigation methods
const nextStep = () => {
  if (canProceedToNext.value && currentStep.value < 4) {
    currentStep.value++
  }
}

const previousStep = () => {
  if (currentStep.value > 1) {
    currentStep.value--
  }
}

const proceedToPayment = () => {
  if (canCompleteBooking.value) {
    currentStep.value = 5
  }
}

const completeBooking = () => {
  // Handle booking completion
  currentStep.value = 5
}

const cancelBooking = () => {
  if (confirm('Are you sure you want to cancel this booking?')) {
    currentPage.value = 'cinemas'
  }
}

const downloadReceipt = () => {
  const referenceNumber = 'BK' + Math.random().toString(36).substr(2, 9).toUpperCase()
  const receiptContent = `
BOOKING RECEIPT
===============================================

Reference Number: ${referenceNumber}

Movie: ${selectedMovie.value.title}
Date & Time: ${selectedDate.value} at ${selectedTime.value}
Cinema: ${selectedCinema.value.name} - Cinema 2

TICKETS
-------
${ticketQty.value['2d'] > 0 ? `2D Regular: ${ticketQty.value['2d']} x ₱330.00 = ₱${(ticketQty.value['2d'] * 330).toFixed(2)}\n` : ''}${ticketQty.value['3d'] > 0 ? `3D Regular: ${ticketQty.value['3d']} x ₱380.00 = ₱${(ticketQty.value['3d'] * 380).toFixed(2)}\n` : ''}${ticketQty.value['imax'] > 0 ? `IMAX: ${ticketQty.value['imax']} x ₱450.00 = ₱${(ticketQty.value['imax'] * 450).toFixed(2)}\n` : ''}
SEATS: ${selectedSeats.value.join(', ')}

${getConcessionTotal() > 0 ? `CONCESSIONS\n-----------\n${concessionItems.value.map(item => {
    if (concessionQty.value[item.id] > 0) {
      return `${item.name}: ${concessionQty.value[item.id]} x ₱${item.price} = ₱${(concessionQty.value[item.id] * item.price).toFixed(2)}`
    }
    return ''
  }).filter(line => line).join('\n')}\n\n` : ''}
PRICING SUMMARY
---------------
Ticket Cost:        ₱${getTicketTotal().toFixed(2)}
${getConcessionTotal() > 0 ? `Concession Cost:    ₱${getConcessionTotal().toFixed(2)}\n` : ''}Booking Fee:        ₱50.00
TOTAL:              ₱${(getTicketTotal() + getConcessionTotal() + 50).toFixed(2)}

===============================================
Customer Name: ${personalDetails.value.name}
Email: ${personalDetails.value.email}

Please present this reference number at the cinema counter
when collecting your tickets.

Thank you for your booking!
`

  // Create a blob and download
  const element = document.createElement('a')
  element.setAttribute('href', 'data:text/plain;charset=utf-8,' + encodeURIComponent(receiptContent))
  element.setAttribute('download', `receipt_${referenceNumber}.txt`)
  element.style.display = 'none'
  document.body.appendChild(element)
  element.click()
  document.body.removeChild(element)
}

const goHome = () => {
  currentPage.value = 'home'
}
</script>

<style scoped>
.booking-section {
  padding: 2rem 0;
  background-color: #f5f5f5;
  min-height: calc(100vh - 80px);
}

.container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 0 2rem;
}

/* Booking Steps Progress */
.booking-steps {
  display: flex;
  justify-content: space-between;
  margin-bottom: 3rem;
  align-items: center;
  gap: 1rem;
}

.step {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
  position: relative;
}

.step::after {
  content: '';
  position: absolute;
  top: 25px;
  left: 50%;
  right: -50%;
  height: 2px;
  background-color: #ddd;
  z-index: -1;
}

.step:last-child::after {
  display: none;
}

.step-circle {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background-color: #f0f0f0;
  color: #999;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 700;
  font-size: 1.1rem;
  transition: all 0.3s ease;
}

.step-label {
  font-size: 0.85rem;
  font-weight: 600;
  color: #999;
  text-align: center;
  transition: all 0.3s ease;
}

.step.active .step-circle {
  background-color: rgb(249, 44, 29);
  color: white;
  box-shadow: 0 4px 12px rgba(249, 44, 29, 0.3);
}

.step.active .step-label {
  color: rgb(249, 44, 29);
  font-weight: 700;
}

.step.completed .step-circle {
  background-color: #4caf50;
  color: white;
}

.step.completed .step-label {
  color: #4caf50;
}

/* Booking Wrapper */
.booking-wrapper {
  display: grid;
  grid-template-columns: 1fr 320px;
  gap: 2rem;
  align-items: start;
}

/* Movie Details Header */
.movie-details-header {
  display: flex;
  gap: 1.5rem;
  padding: 1.5rem;
  background-color: white;
  border-radius: 8px;
  margin-bottom: 1.5rem;
  grid-column: 1 / -1;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.movie-poster-small {
  width: 80px;
  height: 120px;
  object-fit: cover;
  border-radius: 4px;
  flex-shrink: 0;
}

.header-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.header-info h3 {
  margin: 0 0 0.5rem 0;
  font-size: 1.2rem;
  font-weight: 700;
  color: #333;
}

.header-info p {
  margin: 0;
  color: #666;
  font-size: 0.9rem;
}

/* Booking Content */
.booking-content {
  background-color: white;
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  min-height: 400px;
}

.booking-content.full-width {
  grid-column: 1 / -1;
}

.step-content {
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.step-title {
  margin-bottom: 2rem;
  padding-bottom: 1.5rem;
  border-bottom: 2px solid #f0f0f0;
}

.step-title h2 {
  margin: 0 0 0.5rem 0;
  font-size: 1.5rem;
  font-weight: 700;
  color: #333;
}

.step-title p {
  margin: 0;
  color: #666;
  font-size: 0.95rem;
}

/* Tickets Selection */
.tickets-selection {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  margin-bottom: 2rem;
}

/* Ticket Tabs */
.ticket-tabs {
  display: flex;
  gap: 0;
  margin-bottom: 2rem;
  border-bottom: 2px solid #e0e0e0;
}

.tab-btn {
  padding: 1rem 2rem;
  background-color: transparent;
  border: none;
  border-bottom: 3px solid transparent;
  color: #666;
  font-weight: 700;
  font-size: 0.95rem;
  cursor: pointer;
  transition: all 0.3s ease;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.tab-btn:hover {
  color: rgb(249, 44, 29);
}

.tab-btn.active {
  color: rgb(249, 44, 29);
  border-bottom-color: rgb(249, 44, 29);
}

.tab-content {
  animation: slideIn 0.3s ease;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Voucher Container */
.voucher-container {
  display: flex;
  flex-direction: column;
  gap: 2rem;
}

.voucher-input-section {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.voucher-input-section label {
  font-weight: 700;
  font-size: 0.95rem;
  color: #333;
}

.voucher-input-group {
  display: flex;
  gap: 1rem;
}

.voucher-input {
  flex: 1;
  padding: 0.85rem 1rem;
  border: 2px solid #ddd;
  border-radius: 4px;
  font-size: 0.95rem;
  transition: all 0.3s ease;
}

.voucher-input:focus {
  outline: none;
  border-color: rgb(249, 44, 29);
  box-shadow: 0 0 0 3px rgba(249, 44, 29, 0.1);
}

.add-voucher-btn {
  padding: 0.85rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.95rem;
  white-space: nowrap;
}

.add-voucher-btn:hover {
  background-color: #d62828;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(249, 44, 29, 0.3);
}

/* Applied Vouchers */
.applied-vouchers {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.applied-vouchers h4 {
  margin: 0;
  font-size: 1rem;
  font-weight: 700;
  color: #333;
}

.voucher-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.voucher-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  background: linear-gradient(135deg, #fff5f2 0%, #ffe8e3 100%);
  border-left: 4px solid rgb(249, 44, 29);
  border-radius: 4px;
  transition: all 0.3s ease;
}

.voucher-item:hover {
  box-shadow: 0 4px 12px rgba(249, 44, 29, 0.15);
  transform: translateX(4px);
}

.voucher-details {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.voucher-code {
  font-weight: 700;
  color: rgb(249, 44, 29);
  font-size: 0.95rem;
  letter-spacing: 1px;
}

.voucher-type {
  font-size: 0.85rem;
  color: #666;
}

.remove-voucher {
  width: 32px;
  height: 32px;
  border: none;
  background-color: white;
  color: rgb(249, 44, 29);
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
}

.remove-voucher:hover {
  background-color: rgb(249, 44, 29);
  color: white;
}

/* No Vouchers */
.no-vouchers {
  padding: 2rem;
  text-align: center;
  background-color: #f9f9f9;
  border-radius: 8px;
  border: 2px dashed #ddd;
}

.no-vouchers p {
  margin: 0;
  color: #999;
  font-size: 0.95rem;
}

.ticket-row {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 1rem;
  align-items: center;
  padding: 1rem;
  background-color: #f9f9f9;
  border-radius: 6px;
  transition: all 0.3s ease;
}

.ticket-row:hover {
  background-color: #f0f0f0;
}

.ticket-info {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.ticket-name {
  font-weight: 600;
  color: #333;
  font-size: 0.95rem;
}

.ticket-price {
  color: rgb(249, 44, 29);
  font-weight: 700;
  font-size: 1rem;
}

.qty-control {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 0.25rem;
  background-color: white;
}

.qty-btn {
  width: 32px;
  height: 32px;
  border: none;
  background-color: transparent;
  color: rgb(249, 44, 29);
  cursor: pointer;
  font-weight: 700;
  font-size: 1.1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  border-radius: 2px;
}

.qty-btn:hover {
  background-color: rgba(249, 44, 29, 0.1);
}

.qty-control input {
  width: 40px;
  text-align: center;
  border: none;
  font-weight: 600;
  background-color: transparent;
  color: black;
}

.qty-control input:focus {
  outline: none;
}

/* SM Club Section */
.sm-club-section {
  background: linear-gradient(135deg, rgb(249, 44, 29) 0%, rgb(220, 30, 15) 100%);
  color: white;
  padding: 2rem;
  border-radius: 8px;
  text-align: center;
}

.sm-club-section h4 {
  margin: 0 0 0.5rem 0;
  font-size: 1.1rem;
  font-weight: 700;
}

.sm-club-section p {
  margin: 0 0 1rem 0;
  opacity: 0.95;
}

.login-btn {
  background-color: white;
  color: rgb(249, 44, 29);
  border: none;
  padding: 0.75rem 2rem;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.95rem;
}

.login-btn:hover {
  background-color: #f0f0f0;
  transform: translateY(-2px);
}

/* Seats Selection */
.seats-selection {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.5rem;
}

.screen {
  display: inline-block;
  padding: 0.5rem 2rem;
  background-color: #333;
  color: white;
  font-weight: 700;
  border-radius: 4px;
  letter-spacing: 2px;
}

.seat-grid {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding: 2rem;
  background-color: #f9f9f9;
  border-radius: 8px;
  max-width: 600px;
}

.seat-row {
  display: flex;
  gap: 0.5rem;
  justify-content: center;
}

.seat {
  width: 32px;
  height: 32px;
  border: 2px solid #ddd;
  border-radius: 4px;
  background-color: white;
  color: #666;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.7rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
}

.seat:hover:not(.occupied) {
  border-color: rgb(249, 44, 29);
  background-color: #fff0e8;
}

.seat.selected {
  background-color: rgb(249, 44, 29);
  color: white;
  border-color: rgb(249, 44, 29);
}

.seat.occupied {
  background-color: #e0e0e0;
  color: #999;
  cursor: not-allowed;
  border-color: #ccc;
}

.seat-legend {
  display: flex;
  gap: 2rem;
  justify-content: center;
  font-size: 0.85rem;
  color: #666;
}

.legend-item {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.seat-dot {
  width: 16px;
  height: 16px;
  border-radius: 2px;
  border: 1px solid #ddd;
}

.seat-dot.available {
  background-color: white;
}

.seat-dot.selected {
  background-color: rgb(249, 44, 29);
  border-color: rgb(249, 44, 29);
}

.seat-dot.occupied {
  background-color: #e0e0e0;
  border-color: #ccc;
}

.seat-notice {
  color: #999;
  font-size: 0.85rem;
  margin: 0;
  text-align: center;
}

/* Concessions Grid */
.concessions-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 1.5rem;
}

.concession-card {
  padding: 1rem;
  background-color: #f9f9f9;
  border-radius: 8px;
  text-align: center;
  transition: all 0.3s ease;
  border: 2px solid transparent;
}

.concession-card:hover {
  border-color: rgb(249, 44, 29);
  background-color: #fff0e8;
}

.concession-image {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.concession-card h4 {
  margin: 0 0 0.5rem 0;
  font-size: 0.95rem;
  font-weight: 600;
  color: #333;
}

.concession-price {
  margin: 0 0 1rem 0;
  color: rgb(249, 44, 29);
  font-weight: 700;
  font-size: 1rem;
}

.concession-qty {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.25rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 0.25rem;
  background-color: white;
}

.qty-display {
  width: 40px;
  text-align: center;
  font-weight: 600;
  color: #333;
}

/* Order Review */
.order-review {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.review-section {
  padding: 1.5rem;
  background-color: #f9f9f9;
  border-radius: 8px;
  border-left: 4px solid rgb(249, 44, 29);
}

.review-section h4 {
  margin: 0 0 1rem 0;
  font-size: 1rem;
  font-weight: 700;
  color: #333;
}

.review-items {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.review-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 0;
  border-bottom: 1px solid #e0e0e0;
  font-size: 0.95rem;
}

.review-item:last-child {
  border-bottom: none;
}

.review-item span:last-child {
  font-weight: 600;
  color: rgb(249, 44, 29);
}

/* Booking Sidebar */
.booking-sidebar {
  background-color: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  height: fit-content;
  position: sticky;
  top: 100px;
}

.summary-header {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 1.5rem;
  margin: 0;
}

.summary-header h3 {
  margin: 0;
  font-size: 1.1rem;
  font-weight: 700;
}

.summary-content {
  padding: 1.5rem;
  flex: 1;
}

.summary-section {
  padding: 1rem 0;
  border-bottom: 1px solid #e0e0e0;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.summary-section:last-of-type {
  border-bottom: none;
}

.summary-label {
  color: #666;
  font-size: 0.9rem;
  font-weight: 600;
}

.summary-amount {
  color: #333;
  font-weight: 700;
  font-size: 0.95rem;
}

.summary-divider {
  height: 2px;
  background-color: #f0f0f0;
  margin: 1rem 0;
}

.price-breakdown {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding: 1rem 0;
}

.breakdown-item {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: #666;
}

.breakdown-item.total {
  border-top: 2px solid #f0f0f0;
  padding-top: 0.75rem;
  font-size: 1rem;
  font-weight: 700;
  color: #333;
}

.breakdown-item span:last-child {
  text-align: right;
}

.booking-note {
  margin: 1rem 0 0 0;
  font-size: 0.75rem;
  color: #999;
}

.sidebar-actions {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding: 1.5rem;
  border-top: 1px solid #e0e0e0;
  margin-top: auto;
}

.action-btn {
  padding: 0.85rem 1rem;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.95rem;
}

.back-btn {
  background-color: white;
  border: 2px solid #ddd;
  color: #333;
}

.back-btn:hover {
  border-color: rgb(249, 44, 29);
  color: rgb(249, 44, 29);
}

.next-btn {
  background-color: rgb(249, 44, 29);
  color: white;
}

.next-btn:hover:not(:disabled) {
  background-color: #d62828;
  transform: translateY(-2px);
}

.next-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.proceed-btn {
  background-color: #4caf50;
  color: white;
}

.proceed-btn:hover {
  background-color: #45a049;
  transform: translateY(-2px);
}

/* Confirm Step */
.confirm-step {
  padding: 0 !important;
}

.confirm-wrapper {
  display: grid;
  grid-template-columns: 1fr 350px;
  gap: 2rem;
}

.confirm-content {
  display: flex;
  flex-direction: column;
  gap: 2rem;
  padding: 2rem;
}

.form-section {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
}

.form-section h3 {
  margin: 0;
  font-size: 1.2rem;
  font-weight: 700;
  color: #333;
  border-bottom: 2px solid rgb(249, 44, 29);
  padding-bottom: 0.75rem;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.form-group label {
  font-weight: 600;
  color: #333;
  font-size: 0.9rem;
}

.form-input {
  padding: 0.85rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.9rem;
  font-family: inherit;
  transition: all 0.3s ease;
  background-color: white;
}

.form-input:focus {
  outline: none;
  border-color: rgb(249, 44, 29);
  box-shadow: 0 0 0 3px rgba(249, 44, 29, 0.1);
}

.form-input.textarea {
  resize: vertical;
  min-height: 80px;
}

/* Payment Options */
.payment-options {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.payment-option {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1rem;
  background-color: #f9f9f9;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.payment-option:hover {
  background-color: #f0f0f0;
}

.payment-option input[type="radio"] {
  cursor: pointer;
  width: 20px;
  height: 20px;
}

.payment-label {
  font-weight: 600;
  color: #333;
}

/* Payment Note */
.payment-note {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
  background-color: #fff5f2;
  border-left: 4px solid rgb(249, 44, 29);
  border-radius: 4px;
}

.note-text {
  margin: 0;
  font-size: 0.85rem;
  color: #666;
  line-height: 1.5;
}

.checkbox-label {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  font-size: 0.85rem;
  color: #666;
  cursor: pointer;
}

.checkbox-label input[type="checkbox"] {
  margin-top: 2px;
  cursor: pointer;
  width: 16px;
  height: 16px;
}

.highlight {
  color: rgb(249, 44, 29);
  font-weight: 600;
  cursor: pointer;
  text-decoration: underline;
}

.important-note {
  margin: 1rem 0 0 0;
  font-size: 0.85rem;
  font-weight: 600;
  color: #333;
}

.confirm-actions {
  display: flex;
  gap: 1rem;
  margin-top: 1rem;
}

.btn-cancel {
  flex: 1;
  padding: 0.85rem;
  background-color: white;
  border: 2px solid #ddd;
  border-radius: 4px;
  color: #333;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn-cancel:hover {
  border-color: rgb(249, 44, 29);
  color: rgb(249, 44, 29);
}

/* Order Summary Sidebar */
.order-summary-sidebar {
  padding: 0;
}

.summary-box {
  background-color: white;
  border-radius: 8px;
  padding: 0;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  position: sticky;
  top: 100px;
  overflow: hidden;
}

.time-remaining {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 1rem;
  text-align: right;
  font-weight: 600;
  font-size: 0.9rem;
}

.timer {
  font-size: 1.2rem;
  font-weight: 700;
}

.basket-section {
  padding: 1.5rem;
  border-bottom: 1px solid #e0e0e0;
}

.basket-section h4 {
  margin: 0 0 1rem 0;
  font-size: 1rem;
  font-weight: 700;
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 0.75rem;
  margin: -1.5rem -1.5rem 1rem -1.5rem;
  padding: 0.75rem 1.5rem;
}

/* Gift Card Section */
.gift-card-section {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  margin-bottom: 1.5rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid #e0e0e0;
}

.gift-card-input {
  padding: 0.65rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 0.85rem;
}

.balance-label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 0.85rem;
  color: #666;
  cursor: pointer;
}

.balance-label input[type="checkbox"] {
  cursor: pointer;
  width: 14px;
  height: 14px;
}

.btn-apply {
  padding: 0.65rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  cursor: pointer;
  font-size: 0.85rem;
  transition: all 0.3s ease;
}

.btn-apply:hover {
  background-color: #d62828;
}

/* Movie Item */
.movie-item {
  margin-bottom: 1.5rem;
  padding: 1rem;
  background-color: rgb(249, 44, 29);
  border-radius: 4px;
  color: white;
}

.movie-item h5 {
  margin: 0 0 0.5rem 0;
  font-size: 0.95rem;
  font-weight: 700;
}

.movie-details {
  margin: 0;
  font-size: 0.8rem;
  opacity: 0.9;
}

/* Items Table */
.items-table {
  margin-bottom: 1.5rem;
}

.table-header {
  display: grid;
  grid-template-columns: 1.5fr 1fr 0.5fr 1fr;
  gap: 0.5rem;
  padding: 0.75rem;
  background-color: #f9f9f9;
  font-weight: 600;
  font-size: 0.8rem;
  color: rgb(249, 44, 29);
  border-radius: 4px;
}

.table-row {
  display: grid;
  grid-template-columns: 1.5fr 1fr 0.5fr 1fr;
  gap: 0.5rem;
  padding: 0.75rem;
  border-bottom: 1px solid #e0e0e0;
  font-size: 0.85rem;
  align-items: center;
}

.col-item {
  font-weight: 600;
  color: #333;
}

.col-cost,
.col-qty,
.col-subtotal {
  text-align: right;
}

.col-subtotal {
  font-weight: 600;
  color: rgb(249, 44, 29);
}

/* Totals Section */
.totals-section {
  padding-top: 1rem;
  border-top: 2px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.total-row {
  display: flex;
  justify-content: space-between;
  font-size: 0.9rem;
  color: #333;
}

.total-row.grand-total {
  font-weight: 700;
  font-size: 1rem;
  padding-top: 0.5rem;
  color: rgb(249, 44, 29);
  border-top: 1px solid #e0e0e0;
}

/* Success Step */
.success-step {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem !important;
}

.success-container {
  max-width: 600px;
  text-align: center;
}

.success-icon {
  width: 80px;
  height: 80px;
  margin: 0 auto 2rem;
  background-color: #4caf50;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  font-weight: 700;
}

.success-container h2 {
  margin: 0 0 1rem 0;
  font-size: 2rem;
  font-weight: 700;
  color: #333;
}

.success-message {
  margin: 0 0 2rem 0;
  font-size: 0.95rem;
  color: #666;
  line-height: 1.6;
}

.booking-confirmation-card {
  background-color: #f9f9f9;
  border: 2px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  margin-bottom: 2rem;
  text-align: left;
}

.confirmation-header {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 1.5rem;
}

.confirmation-header h3 {
  margin: 0 0 1rem 0;
  font-size: 1.3rem;
  font-weight: 700;
}

.confirmation-details p {
  margin: 0.5rem 0;
  font-size: 0.9rem;
}

.confirmation-body {
  padding: 1.5rem;
}

.confirmation-body h4 {
  margin: 0 0 1rem 0;
  font-size: 1rem;
  font-weight: 700;
  color: #333;
}

.summary-table {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.summary-row {
  display: flex;
  justify-content: space-between;
  padding: 0.75rem 0;
  border-bottom: 1px solid #e0e0e0;
  font-size: 0.9rem;
}

.summary-row.total {
  border-top: 2px solid rgb(249, 44, 29);
  font-weight: 700;
  color: rgb(249, 44, 29);
  font-size: 1rem;
}

.confirmation-footer {
  padding: 1rem 1.5rem;
  background-color: #f0f0f0;
  font-size: 0.85rem;
  color: #666;
}

.confirmation-footer p {
  margin: 0.5rem 0;
}

.success-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
}

.btn-download,
.btn-home {
  padding: 0.85rem 2rem;
  border: none;
  border-radius: 4px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  font-size: 0.95rem;
}

.btn-download {
  background-color: white;
  border: 2px solid rgb(249, 44, 29);
  color: rgb(249, 44, 29);
}

.btn-download:hover {
  background-color: rgb(249, 44, 29);
  color: white;
}

.btn-home {
  background-color: rgb(249, 44, 29);
  color: white;
}

.btn-home:hover {
  background-color: #d62828;
  transform: translateY(-2px);
}

/* Responsive */
@media (max-width: 1024px) {
  .booking-wrapper {
    grid-template-columns: 1fr;
  }

  .booking-sidebar {
    position: static;
  }

  .tickets-selection {
    grid-template-columns: 1fr;
  }

  .concessions-grid {
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  }
}

@media (max-width: 768px) {
  .booking-steps {
    flex-direction: column;
    gap: 0.5rem;
  }

  .step::after {
    display: none;
  }

  .step-circle {
    width: 40px;
    height: 40px;
    font-size: 0.9rem;
  }

  .step-label {
    font-size: 0.75rem;
  }

  .movie-details-header {
    flex-direction: column;
    text-align: center;
  }

  .movie-poster-small {
    width: 100px;
    height: 150px;
  }

  .booking-content {
    padding: 1.5rem;
  }

  .tickets-selection {
    grid-template-columns: 1fr;
  }

  .seat-grid {
    max-width: 100%;
    overflow-x: auto;
  }

  .concessions-grid {
    grid-template-columns: repeat(3, 1fr);
  }

  .seat-legend {
    flex-direction: column;
    gap: 0.5rem;
  }
}
</style>
