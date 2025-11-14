<template>
  <section class="contact-section">
    <div class="container">
      <button class="back-btn" @click="goBack">← Back</button>
      
      <div class="contact-wrapper">
        <!-- Contact Info -->
        <div class="contact-info">
          <h2>CONTACT US</h2>
          
          <div class="info-block">
            <h3>Corporate Address</h3>
            <p>SM Cinemas, 2/F SM MOA Complex, Mall of Asia Avenue City, Coral Way cor. J.W. Diokno Blvd, Pasay City, Philippines</p>
          </div>

          <div class="info-block">
            <h3>Customer Care</h3>
            <p>Email: <a href="mailto:customer.care@smcinemas.com">customer.care@smcinemas.com</a></p>
          </div>
        </div>

        <!-- Contact Form -->
        <div class="contact-form-wrapper">
          <h2>CONTACT FORM</h2>
          
          <form @submit.prevent="submitForm" class="contact-form">
            <div class="form-group">
              <label for="name">NAME *</label>
              <input 
                type="text" 
                id="name" 
                v-model="formData.name" 
                required
                placeholder="Enter your name"
              />
            </div>

            <div class="form-group">
              <label for="email">EMAIL ADDRESS *</label>
              <input 
                type="email" 
                id="email" 
                v-model="formData.email" 
                required
                placeholder="Enter your email"
              />
            </div>

            <div class="form-group">
              <label for="subject">SUBJECT *</label>
              <select id="subject" v-model="formData.subject" required>
                <option value="">Select Subject</option>
                <option value="general">General Inquiry</option>
                <option value="complaint">Complaint</option>
                <option value="suggestion">Suggestion</option>
                <option value="event">Event Inquiry</option>
                <option value="other">Other</option>
              </select>
            </div>

            <div class="form-group">
              <label for="category">SUB-CATEGORY *</label>
              <select id="category" v-model="formData.category" required>
                <option value="">Select Category</option>
                <option value="ticket">Ticket Issue</option>
                <option value="facility">Facility</option>
                <option value="service">Service Quality</option>
                <option value="booking">Booking</option>
                <option value="other">Other</option>
              </select>
            </div>

            <div class="form-group">
              <label for="message">MESSAGE *</label>
              <textarea 
                id="message" 
                v-model="formData.message" 
                required
                placeholder="Enter your message"
                rows="6"
              ></textarea>
            </div>

            <div class="form-disclaimer">
              <label>
                <input type="checkbox" v-model="formData.agreeTerms" required />
                By providing your personal data and clicking 'Send Message', you acknowledge that you have read our 
                <a href="#">Data Privacy Policy</a>
              </label>
            </div>

            <button type="submit" class="submit-btn">SEND MESSAGE</button>
          </form>

          <div v-if="submitMessage" :class="['submit-message', submitMessage.type]">
            {{ submitMessage.text }}
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from 'vue'

const formData = ref({
  name: '',
  email: '',
  subject: '',
  category: '',
  message: '',
  agreeTerms: false
})

const submitMessage = ref(null)

const emit = defineEmits(['navigate'])

const goBack = () => {
  emit('navigate', 'cinemas')
}

const submitForm = () => {
  if (!formData.value.agreeTerms) {
    submitMessage.value = {
      type: 'error',
      text: 'Please agree to the Data Privacy Policy'
    }
    return
  }

  // Here you would typically send the form data to a backend
  console.log('Form submitted:', formData.value)
  
  submitMessage.value = {
    type: 'success',
    text: 'Thank you! Your message has been sent successfully. We will get back to you soon.'
  }

  // Reset form after 2 seconds
  setTimeout(() => {
    formData.value = {
      name: '',
      email: '',
      subject: '',
      category: '',
      message: '',
      agreeTerms: false
    }
    submitMessage.value = null
  }, 2000)
}
</script>

<style scoped>
.contact-section {
  padding: 3rem 0;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.back-btn {
  display: inline-block;
  margin-bottom: 2rem;
  padding: 0.75rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.back-btn:hover {
  background-color: #D62828;
}

.contact-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 3rem;
  margin-bottom: 3rem;
}

.contact-info h2,
.contact-form-wrapper h2 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 2rem;
  color: #333;
}

.info-block {
  margin-bottom: 2rem;
}

.info-block h3 {
  font-size: 1rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.5rem;
}

.info-block p {
  color: #666;
  line-height: 1.6;
  margin: 0;
}

.info-block a {
  color: #E63946;
  text-decoration: none;
  font-weight: 600;
}

.info-block a:hover {
  text-decoration: underline;
}

.contact-form {
  background-color: white;
  padding: 2rem;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.form-group {
  margin-bottom: 1.5rem;
}

.form-group label {
  display: block;
  font-weight: 600;
  color: #333;
  margin-bottom: 0.5rem;
  font-size: 0.95rem;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 1rem;
  font-family: inherit;
  transition: border-color 0.3s ease;
  background-color: white;
}

.form-group input:focus,
.form-group select:focus,
.form-group textarea:focus {
  outline: none;
  border-color: #E63946;
  box-shadow: 0 0 0 3px rgba(230, 57, 70, 0.1);
}

.form-group textarea {
  resize: vertical;
}

.form-disclaimer {
  margin-bottom: 1.5rem;
  padding: 1rem;
  background-color: #f9f9f9;
  border-radius: 4px;
}

.form-disclaimer label {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  margin: 0;
  font-weight: 400;
  color: #666;
}

.form-disclaimer input[type="checkbox"] {
  width: auto;
  margin-top: 0.25rem;
  cursor: pointer;
}

.form-disclaimer a {
  color: #E63946;
  text-decoration: none;
  font-weight: 600;
}

.form-disclaimer a:hover {
  text-decoration: underline;
}

.submit-btn {
  width: 100%;
  padding: 0.75rem 1.5rem;
  background-color: rgb(249, 44, 29);
  color: white;
  border: none;
  border-radius: 4px;
  font-weight: 600;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-btn:hover {
  background-color: #D62828;
}

.submit-message {
  margin-top: 1rem;
  padding: 1rem;
  border-radius: 4px;
  text-align: center;
  font-weight: 600;
}

.submit-message.success {
  background-color: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

.submit-message.error {
  background-color: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

@media (max-width: 768px) {
  .contact-wrapper {
    grid-template-columns: 1fr;
    gap: 2rem;
  }

  .contact-form {
    padding: 1.5rem;
  }
}
</style>
