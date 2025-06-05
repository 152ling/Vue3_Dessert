<template>
    <div class="product-list">
      <h2>Desserts</h2>
      <div class="product-grid">
      <div
      class="product-item"
      v-for="item in products"
      :key="item.id"
    >
          <div class="image-container">
            <picture>
              <source :srcset="item.image.desktop" media="(min-width: 1024px)" />
              <source :srcset="item.image.tablet" media="(min-width: 768px)" />
              <source :srcset="item.image.mobile" media="(max-width: 767px)" />
              <img :src="item.image.thumbnail" :alt="item.name" class="product-image" />
            </picture>
            <button class="image-btn" @click="addToCart(item)">Add to Cart</button>
          </div>

          <div class="product-info">
            <p class="product-category">{{ item.category}}</p>
            <p class="product-name">{{ item.name }}</p>
            <p class="product-price">${{ item.price.toFixed(2) }}</p>
          </div>
        </div>
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
  .product-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    }
  .product-item {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 16px;
    padding: 10px;
    border-radius: 8px;
  }
  .image-container {
    position: relative;
    width: 100%;
  }

  .product-image {
    display: block;
    width: 220px;
    height: 220px;
    object-fit: cover;
    border-radius: 6px;
  }
  .image-btn {
    position: absolute;
    bottom: -10px;
    left: 50%;
    transform: translateX(-50%);
    width: 150px;
    background-color: white;
    color: black;
    border: 1px solid black;
    border-radius: 30px;
    cursor: pointer;
    font-size: 14px;
  }
  .image-btn:hover{
    border: 1px solid #ff6f61;
    color:#ff6f61
  }

  .product-info {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    width: 220px;
    text-align: left;
  }
  .product-info p{
    margin: 0px;
  }
  .product-category{
    color: #888;
  }

  .product-name {
    font-weight: bold;
  }

  .product-price {
    color: #cc3d3d;
  }


  /* .product-image{
    width:200px;
  }
  .product-list {
    flex: 2;
    padding-right: 20px;
    border-right: 1px solid #ccc;
  }
  
  .product-item {
    margin-bottom: 10px;
  } */
  </style>
  