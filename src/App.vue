<template>
  <div class="app-container">
    <ProductList :products="products" :addToCart="addToCart" :decreaseQuantity="decreaseQuantity" :cart="cart"/>

    <div class="cart">
      <h2 style="color:#C13C12"><b>Your Cart({{totalquantity}})</b></h2>
      <!-- 顯示「空的購物車」畫面 -->
      <div v-if="totalquantity === 0" style="color: #888;display: flex;flex-direction: column;align-items: center;">
        <img :src="emptycart" alt="emptycart" style="width: min-content;">
        <p>Your added items will appear here</p>
      </div>

      <!-- 顯示有商品的購物車列表 -->
      <div v-else>
        <ul>
          <li v-for="item in cart" :key="item.id" class="cart-item">
            <div class="item-info">
              <b>{{ item.name }}</b>
              <div style="display:flex;gap:10px">
                <p style="color:#C13C12">{{ item.quantity }}x</p>
                <p style="color:#B9B1AC">@ ${{ item.price.toFixed(2) }}</p>
                <p style="color:#807070">${{ (item.price*item.quantity).toFixed(2) }}</p>
              </div>
            </div>
            <div class="item-remove">
              <button class="remove-icon" @click="removeFromCart(item)">
                <svg width="10" height="10" fill="none" viewBox="0 0 10 10"><path fill="#CAAFA7" d="M8.375 9.375 5 6 1.625 9.375l-1-1L4 5 .625 1.625l1-1L5 4 8.375.625l1 1L6 5l3.375 3.375-1 1Z"/></svg>
              </button></div>
          </li>
        </ul>
        <div class="cart-price" style="display: flex;justify-content: space-between;align-items: center;margin:5px 0px;">
          <p>Order Total</p>
          <h3><b>${{ totalPrice.toFixed(2) }}</b></h3>
        </div>
        <div class="memo">
            <svg width="21" height="20" fill="none" viewBox="0 0 21 20"><path fill="#1EA575" d="M8 18.75H6.125V17.5H8V9.729L5.803 8.41l.644-1.072 2.196 1.318a1.256 1.256 0 0 1 .607 1.072V17.5A1.25 1.25 0 0 1 8 18.75Z"/><path fill="#1EA575" d="M14.25 18.75h-1.875a1.25 1.25 0 0 1-1.25-1.25v-6.875h3.75a2.498 2.498 0 0 0 2.488-2.747 2.594 2.594 0 0 0-2.622-2.253h-.99l-.11-.487C13.283 3.56 11.769 2.5 9.875 2.5a3.762 3.762 0 0 0-3.4 2.179l-.194.417-.54-.072A1.876 1.876 0 0 0 5.5 5a2.5 2.5 0 1 0 0 5v1.25a3.75 3.75 0 0 1 0-7.5h.05a5.019 5.019 0 0 1 4.325-2.5c2.3 0 4.182 1.236 4.845 3.125h.02a3.852 3.852 0 0 1 3.868 3.384 3.75 3.75 0 0 1-3.733 4.116h-2.5V17.5h1.875v1.25Z"/></svg>
            <p>This is a <b>carbon-neutral</b> delivery</p>
        </div>
        <!-- <button class="submit-btn" >Confirm Order</button> -->
        <!-- Button trigger modal -->
        <button type="button" class="submit-btn" data-bs-toggle="modal" data-bs-target="#exampleModal">
          Confirm Order
        </button>

        <!-- Modal -->
        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog">
            <div class="modal-content">
              <div class="modal-header">
                <div class="row">
                  <svg width="48" height="48" viewBox="0 0 48 48" fill="none" style="width: 60px;">
                    <path d="M21 32.121L13.5 24.6195L15.6195 22.5L21 27.879L32.3775 16.5L34.5 18.6225L21 32.121Z" fill="#1EA575"/>
                    <path d="M24 3C19.8466 3 15.7865 4.23163 12.333 6.53914C8.8796 8.84665 6.18798 12.1264 4.59854 15.9636C3.0091 19.8009 2.59323 24.0233 3.40352 28.0969C4.21381 32.1705 6.21386 35.9123 9.15077 38.8492C12.0877 41.7861 15.8295 43.7862 19.9031 44.5965C23.9767 45.4068 28.1991 44.9909 32.0364 43.4015C35.8736 41.812 39.1534 39.1204 41.4609 35.667C43.7684 32.2135 45 28.1534 45 24C45 18.4305 42.7875 13.089 38.8493 9.15076C34.911 5.21249 29.5696 3 24 3ZM24 42C20.4399 42 16.9598 40.9443 13.9997 38.9665C11.0397 36.9886 8.73256 34.1774 7.37018 30.8883C6.0078 27.5992 5.65134 23.98 6.34587 20.4884C7.04041 16.9967 8.75474 13.7894 11.2721 11.2721C13.7894 8.75473 16.9967 7.0404 20.4884 6.34587C23.98 5.65133 27.5992 6.00779 30.8883 7.37017C34.1774 8.73255 36.9886 11.0397 38.9665 13.9997C40.9443 16.9598 42 20.4399 42 24C42 28.7739 40.1036 33.3523 36.7279 36.7279C33.3523 40.1036 28.7739 42 24 42Z" fill="#1EA575"/>
                  </svg>
                  <h2 class="modal-title " id="exampleModalLabel"><b>Order Confirm</b></h2>
                  <p>We hope you enjoy your food!</p>
                </div>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <ul>
                  <li v-for="item in cart" :key="item.id" class="cart-item">
                    <div class="confirm-item-info d-flex w-100 justify-content-between">
                      <div class="left-side d-flex gap-2" style="flex:5;" >
                        <picture>
                          <source :srcset="item.image.desktop" media="(min-width: 1024px)" />
                          <source :srcset="item.image.tablet" media="(min-width: 768px)" />
                          <source :srcset="item.image.mobile" media="(max-width: 767px)" />
                          <img :src="item.image.thumbnail" :alt="item.name" class="confirm-image"/>
                        </picture>
                        <div class="row">
                          <span>{{ item.name }}</span>
                          <div class="d-flex gap-2">
                            <span style="color:#C13C12">{{ item.quantity }}x</span>
                            <span style="color:#B9B1AC">@ ${{ item.price.toFixed(2) }}</span>
                          </div>
                        </div>
                      </div>
                      <div class="right-side"  style="flex:1">
                        <p style="color:#807070">${{ (item.price*item.quantity).toFixed(2) }}</p>
                      </div>
                                   
                    </div>
                  </li>
                </ul>
                <div class="cart-price" style="display: flex;justify-content:space-between;align-items: center;">
                  <p>Order Total</p>
                  <h2>${{ totalPrice.toFixed(2)}}</h2>
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn submit-btn" data-bs-dismiss="modal">Start New Order</button>
              </div>
            </div>
          </div>
        </div> <!-- Modal -->

      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ProductList from './components/ProductList.vue'
import { products as productData } from './products.js'
const products = ref(productData)
import emptycart from '@/assets/images/illustration-empty-cart.svg'

const cart = ref([])

const addToCart = (item) => {
  const existingItem = cart.value.find((cartItem) => cartItem.id === item.id);
  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cart.value.push({ ...item, quantity: 1 });
  }
}
const decreaseQuantity = (item) => {
  // const found = cart.find(i => i.id === item.id)
  const found = cart.value.find((cartItem) => cartItem.id === item.id);
  if (found) {
    if (found.quantity > 1) {
      found.quantity -= 1
    } else {
      // 數量為 1 點減號時，移出購物車
      const index = cart.value.findIndex(i => i.id === item.id)
      cart.value.splice(index, 1)
    }
  }
}
const removeFromCart=(item)=>{
  const index = cart.value.findIndex(i => i.id === item.id)
  cart.value.splice(index, 1)
}

const getQuantity = (id) => {
  const found = cart.value.find(item => item.id === id)
  return found ? found.quantity : 0
}

const totalPrice = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.price*item.quantity, 0)
})
const totalquantity = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.quantity, 0)
})
</script>

<style scoped>
.app-container {
  display: flex;
  flex-direction: row; /* 預設小螢幕直向排列 */
  padding: 20px;
  font-family: sans-serif;
}
p,h1{
  margin: 0;
}
 /* Medium devices (tablets, 768px and up) */
@media (max-width: 768px) {
  .app-container {
    flex-direction:column;
    padding: 20px;
  }
  .cart {
    width: 90%;
    height: max-content;
    padding: 20px;
    text-decoration: none;
    background-color: white;
    border-radius: 20px;
    margin:0  auto;
  }

}

.memo{
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  color: #4C4A4A;
  margin:auto;
  padding:10px 25px;
  border-radius: 10px;
  background-color: #fcf8f5;
}
.memo p{
  margin: 0;
}

.product-list {
  flex: 2;
}

.product-item {
  margin-bottom: 10px;
}


.cart{
    flex: 1;
    margin-top: 20px;
    height: max-content;
    padding: 20px;
    text-decoration: none;
    background-color: white;
    border-radius: 20px;
  }
.cart ul{
  list-style-type: none;
  padding: 0px;
}
.cart-item{
  display: flex;
  justify-content: space-between;
  width: 100%;
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid gray;
}

.modal-body{
  background-color: #fcf8f5;
  margin: 20px;
}
.confirm-image {
    display: block;
    width: 60px;
    height: 60px;
    object-fit: cover;
    border-radius: 6px;
  }
.remove-icon{
  display:flex;
  align-items: center;
  justify-content: center;
  height: 20px;
  width: 20px;
  border: 1.5px solid #CAAFA7 ;
  background-color: transparent;
  padding:0;
  border-radius:50%;
  cursor: pointer;
}
.submit-btn{
  background-color: #C13C12;
  margin-top: 20px;
  color: white;
  padding: 15px;
  border: none;
  border-radius: 25px;
  width: 100%;
}
</style>
