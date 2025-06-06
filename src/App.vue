<template>
  <div class="app-container">
    <ProductList :products="products" :addToCart="addToCart" :decreaseQuantity="decreaseQuantity" :cart="cart"/>

    <div class="cart">
      <h2 style="color:#ff6f61">Your Cart({{totalquantity}})</h2>
      <!-- 顯示「空的購物車」畫面 -->
      <div v-if="totalquantity === 0" style="color: #888;">
        <img :src="emptycart" alt="emptycart">
        <p>Your added items will appear here</p>
      </div>

      <!-- 顯示有商品的購物車列表 -->
      <div v-else>
      <ul>
        <li v-for="item in cart" :key="item.id" class="cart-item">
          <div class="item-info">
            {{ item.name }}
            <div style="display:flex;gap:10px">
              <p style="color:#ff6f61">{{ item.quantity }}x</p>
              <p style="color:#72604d">@ ${{ item.price.toFixed(2) }}</p>
              <p style="color:#533f2a">${{ item.price*item.quantity }}</p>
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
        <h4>${{ totalPrice }}</h4>
      </div>
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
  padding: 20px;
  font-family: sans-serif;
}

.product-list {
  flex: 2;
  border-right: 1px solid #ccc;
}

.product-item {
  margin-bottom: 10px;
}

.cart {
  flex: 1;
  padding-left: 10px;
  text-decoration: none;
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
</style>
