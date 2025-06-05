<template>
  <div class="app-container">
    <ProductList :products="products" :addToCart="addToCart" />

    <div class="cart">
      <h2>購物車</h2>
      <ul>
        <li v-for="(item, index) in cart" :key="index">
          {{ item.name }}
          ${{ item.price }}
          {{ item.quantity }}
        </li>
      </ul>
      <p>總金額：${{ totalPrice }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ProductList from './components/ProductList.vue'
import { products as productData } from './products.js'
const products = ref(productData)

const cart = ref([])

const addToCart = (item) => {
  const existingItem = cart.value.find((cartItem) => cartItem.id === item.id);
  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    cart.value.push({ ...item, quantity: 1 });
  }
}

const totalPrice = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.price*item.quantity, 0)
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
</style>
