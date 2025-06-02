<template>
  <div class="app-container">
    <ProductList :products="products" :addToCart="addToCart" />

    <div class="cart">
      <h2>購物車</h2>
      <ul>
        <li v-for="(item, index) in cart" :key="index">
          {{ item.name }} - ${{ item.price }}
        </li>
      </ul>
      <p>總金額：${{ totalPrice }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import ProductList from './components/ProductList.vue'

const products = ref([
  { id: 1, name: '草莓蛋糕', price: 100 },
  { id: 2, name: '巧克力布朗尼', price: 120 },
  { id: 3, name: '檸檬塔', price: 90 }
])

const cart = ref([])

const addToCart = (item) => {
  cart.value.push(item)
}

const totalPrice = computed(() => {
  return cart.value.reduce((sum, item) => sum + item.price, 0)
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
  padding-right: 20px;
  border-right: 1px solid #ccc;
}

.product-item {
  margin-bottom: 10px;
}

.cart {
  flex: 1;
  padding-left: 20px;
}
</style>
