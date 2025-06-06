<template>
  <div class="app-container">
    <ProductList :products="products" :addToCart="addToCart" :decreaseQuantity="decreaseQuantity" :cart="cart"/>

    <div class="cart">
      <h2 style="color:#C13C12">Your Cart({{totalquantity}})</h2>
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
              {{ item.name }}
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
        <div class="cart-price" style="display: flex;justify-content: space-between;align-items: center;">
          <p>Order Total</p>
          <h2>${{ totalPrice.toFixed(2) }}</h2>
        </div>
        <div class="memo">
            <svg width="21" height="20" fill="none" viewBox="0 0 21 20"><path fill="#1EA575" d="M8 18.75H6.125V17.5H8V9.729L5.803 8.41l.644-1.072 2.196 1.318a1.256 1.256 0 0 1 .607 1.072V17.5A1.25 1.25 0 0 1 8 18.75Z"/><path fill="#1EA575" d="M14.25 18.75h-1.875a1.25 1.25 0 0 1-1.25-1.25v-6.875h3.75a2.498 2.498 0 0 0 2.488-2.747 2.594 2.594 0 0 0-2.622-2.253h-.99l-.11-.487C13.283 3.56 11.769 2.5 9.875 2.5a3.762 3.762 0 0 0-3.4 2.179l-.194.417-.54-.072A1.876 1.876 0 0 0 5.5 5a2.5 2.5 0 1 0 0 5v1.25a3.75 3.75 0 0 1 0-7.5h.05a5.019 5.019 0 0 1 4.325-2.5c2.3 0 4.182 1.236 4.845 3.125h.02a3.852 3.852 0 0 1 3.868 3.384 3.75 3.75 0 0 1-3.733 4.116h-2.5V17.5h1.875v1.25Z"/></svg>
            <p>This is a <b>carbon-neutral</b> delivery</p>
        </div>
        <button class="submit-btn" >Confirm Order</button>
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
  flex-direction: column; /* 預設小螢幕直向排列 */
  padding: 20px;
  font-family: sans-serif;
}
 /* Medium devices (tablets, 768px and up) */
@media (min-width: 768px) {
  .app-container {
    flex-direction: row;
    padding: 20px;
  }
  .cart {
    flex: 1;
    margin-top: 20px;
    padding-left: 10px;
    text-decoration: none;
    background-color: white;
    border-radius: 20px;
  }

}

.memo{
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 14px;
  color: #4C4A4A;
  margin:auto;
  padding:0px 25px;
  border-radius: 10px;
  background-color: #fcf8f5;
}

.product-list {
  flex: 2;
}

.product-item {
  margin-bottom: 10px;
}


.cart{
    width: 90%;
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
  align-items: center;
  padding: 10px 0;
  border-bottom: 1px solid gray;
}
.remove-icon{
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
