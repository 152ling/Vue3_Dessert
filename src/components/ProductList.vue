<template>
    <div class="product-list">
      <h2>商品列表</h2>
      <div v-for="item in products" :key="item.id" class="product-item">
        <p>{{ item.name }} - ${{ item.price }}</p>
        <button @click="addToCart(item)">加入購物車</button>
      </div>
    </div>
  </template>
  
  <script setup>
  
  defineProps({
    products: Array,
    addToCart: Function
  })
  </script>
  
  <style scoped>
  .product-list {
    flex: 2;
    padding-right: 20px;
    border-right: 1px solid #ccc;
  }
  
  .product-item {
    margin-bottom: 10px;
  }
  </style>
  