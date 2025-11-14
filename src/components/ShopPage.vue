<template>
  <section class="shop-section">
    <div class="container">
      <h1 class="shop-title">SM CINEMAS ONLINE SHOP</h1>

      <div class="shop-wrapper">
        <!-- Left Content - Products -->
        <div class="products-area">
          <div class="products-layout">
            <div class="loyalty-tab">
              <span>Loyalty</span>
            </div>

            <div class="products-container">
              <div class="products-grid">
                <!-- Product 1 -->
                <div class="product-card">
                  <div class="product-image">
                    <img src="https://via.placeholder.com/180x120?text=Cinema+Load+300" alt="Cinema Load Php 300" />
                    <span class="price-badge">₱300</span>
                  </div>
                  <h3>Cinema Load Php 300</h3>
                  <p class="product-price">300.00</p>
                </div>

                <!-- Product 2 -->
                <div class="product-card">
                  <div class="product-image">
                    <img src="https://via.placeholder.com/180x120?text=Cinema+Load+500" alt="Cinema Load Php 500" />
                    <span class="price-badge">₱500</span>
                  </div>
                  <h3>Cinema Load Php 500</h3>
                  <p class="product-price">500.00</p>
                </div>

                <!-- Product 3 -->
                <div class="product-card">
                  <div class="product-image">
                    <img src="https://via.placeholder.com/180x120?text=Cinema+Load+1000" alt="Cinema Load Php 1000" />
                    <span class="price-badge">₱1000</span>
                  </div>
                  <h3>Cinema Load Php1000</h3>
                  <p class="product-price">1,000.00</p>
                </div>
              </div>

              <div class="shop-notice">
                <p>
                  <button class="register-link" @click="goToLoyalty">Sign in/ Register to SM Cinema Club</button>
                  to purchase Cinema Gift Cards
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Right Content - Purchase Summary -->
        <div class="purchase-summary">
          <div class="summary-header">
            <h2>PURCHASE SUMMARY</h2>
          </div>

          <div class="summary-content">
            <p class="empty-message">Your cart is empty</p>
          </div>

          <div class="summary-footer">
            <div class="total-cost">
              <span>Total Cost</span>
              <span class="amount">₱0.00</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
const emit = defineEmits(['navigate'])

const goToLoyalty = () => {
  emit('navigate', 'loyalty')
}
</script>

<style scoped>
.shop-section {
  padding: 3rem 0;
  background-color: #f9f9f9;
  min-height: calc(100vh - 80px);
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.shop-title {
  font-size: 2rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 3rem;
}

.shop-wrapper {
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 2rem;
}

.products-area {
  display: flex;
  flex-direction: column;
}

.products-layout {
  display: flex;
  gap: 0;
  overflow: hidden;
}

.loyalty-tab {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 0.5rem 1rem;
  font-weight: 700;
  font-size: 0.95rem;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  gap: 0.5rem;
  position: relative;
  height: fit-content;
  white-space: nowrap;
}

.loyalty-tab::after {
  content: '';
  position: absolute;
  right: -15px;
  top: 0;
  bottom: 0;
  width: 0;
  height: 0;
  border-left: 15px solid rgb(249, 44, 29);
  border-top: 20px solid transparent;
  border-bottom: 20px solid transparent;
}

.products-container {
  background-color: white;
  padding: 2rem;
  border: 3px solid rgb(249, 44, 29);
  border-radius: 4px;
}

.products-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
  margin-bottom: 2rem;
}

.product-card {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1.5rem;
  text-align: center;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
}

.product-image {
  position: relative;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f5f5f5;
  border-radius: 4px;
  overflow: hidden;
  height: 140px;
}

.product-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.price-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: rgba(0, 0, 0, 0.7);
  color: white;
  padding: 0.5rem 0.75rem;
  border-radius: 4px;
  font-weight: 700;
  font-size: 0.9rem;
}

.product-card h3 {
  font-size: 1rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 0.75rem;
  line-height: 1.4;
}

.product-price {
  font-size: 1.1rem;
  font-weight: 600;
  color: #333;
  margin: 0;
}

.shop-notice {
  background-color: #f9f9f9;
  padding: 1rem;
  border-top: 1px solid #ddd;
  margin-top: 1.5rem;
  text-align: center;
}

.shop-notice p {
  font-size: 0.9rem;
  color: #333;
  margin: 0;
}

.register-link {
  background: none;
  border: none;
  color: rgb(249, 44, 29);
  text-decoration: underline;
  font-weight: 600;
  cursor: pointer;
  padding: 0;
  font-size: 0.9rem;
  font-family: inherit;
}

.register-link:hover {
  color: #D62828;
}

/* Purchase Summary Sidebar */
.purchase-summary {
  background-color: white;
  border: 3px solid rgb(249, 44, 29);
  border-radius: 4px;
  height: fit-content;
  display: flex;
  flex-direction: column;
}

.summary-header {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 1rem 1.5rem;
  font-weight: 700;
}

.summary-header h2 {
  margin: 0;
  font-size: 1.1rem;
}

.summary-content {
  flex: 1;
  padding: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 300px;
}

.empty-message {
  color: #999;
  font-size: 0.95rem;
  text-align: center;
}

.summary-footer {
  border-top: 2px solid #ddd;
}

.total-cost {
  background-color: rgb(249, 44, 29);
  color: white;
  padding: 1rem 1.5rem;
  font-weight: 700;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.amount {
  font-size: 1.1rem;
}

@media (max-width: 1024px) {
  .shop-wrapper {
    grid-template-columns: 1fr;
  }

  .products-layout {
    grid-template-columns: 1fr;
  }

  .loyalty-tab {
    min-width: auto;
  }

  .purchase-summary {
    height: auto;
  }
}

@media (max-width: 768px) {
  .shop-title {
    font-size: 1.5rem;
    margin-bottom: 2rem;
  }

  .products-grid {
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 1rem;
  }

  .products-container {
    padding: 1.5rem;
  }

  .product-card {
    padding: 1rem;
  }

  .product-image {
    height: 100px;
  }

  .product-card h3 {
    font-size: 0.9rem;
  }
}
</style>
