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
              <img :src="item.image.thumbnail" :alt="item.name" class="product-image"  :class="{ 'click-product': getQuantity(item.id) > 0 }"/>
            </picture>
            <button v-if="getQuantity(item.id) === 0" class="image-btn" @click="addToCart(item)">
              <img :src="icon_add_cart" alt="">
              Add to Cart</button>
            <div v-else class="edit-btn">
              <button @click="decreaseQuantity(item)">
                <svg class="icon" viewBox="0 0 10 2" width="10" height="20" fill="currentColor">
                  <path d="M0 .375h10v1.25H0V.375Z"/>
                </svg>
              </button>
              <span>{{ getQuantity(item.id) }}</span>
              <button @click="addToCart(item)">
                <svg class="icon" width="10" height="10" fill="currentColor">
                  <path d="M10 4.375H5.625V0h-1.25v4.375H0v1.25h4.375V10h1.25V5.625H10v-1.25Z"/>
                </svg>
              </button>
            </div>
          </div>

          <div class="product-info">
            <p class="product-category">{{ item.category}}</p>
            <p class="product-name">{{ item.name }}</p>
            <p class="product-price">${{ item.price.toFixed(2) }}</p>
            <!-- <p v-if="getQuantity(item.id) > 0">
              已加入：{{ getQuantity(item.id) }} 件
            </p> -->

          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  const { cart } = defineProps(['products', 'addToCart','decreaseQuantity','cart'])
  import icon_add_cart from '@/assets/images/icon-add-to-cart.svg'
  const getQuantity = (id) => {
    if (!Array.isArray(cart)) return 0;
    const item = cart.find((i) => i.id === id);
    return item ? item.quantity : 0;
  }
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
  }
  .click-product{
    border: 2px solid #C83B10;
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
    height:35px;
    display: flex;
    justify-content: center;
    background-color: white;
    color: black;
    border: 1px solid black;
    border-radius: 30px;
    cursor: pointer;
    font-size: 14px;
    padding: 0.6em 1.2em;
  }
  .image-btn:hover{
    border: 1px solid #C73B0F;
    color:#C73B0F;
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
    color: #C83B10;
  }
  .button-group {
      margin-top: 10px;
    }

  .edit-btn {
    position: absolute;
    bottom: -10px;
    left: 50%;
    transform: translateX(-50%);
    width: 150px;
    height:35px;
    background-color: #C83B10;
    color: white;
    border-radius: 30px;
    display: flex;
    justify-content:space-evenly;
    align-items: center;
  }
  .edit-btn button {
    background-color: transparent;
    border: 1.5px solid white;
    border-radius:50%;
    font-size: 18px;
    color: white;
    cursor: pointer;
    height: 20px;
    width: 20px;
    display:flex;
    align-items: center;
    justify-content: center;
    padding: 0;
  }
  .edit-btn button:hover{
    background-color: white;
  }

  .edit-btn button:hover .icon{
    color:black;
  }
  .edit-btn span {
    min-width: 20px;
    text-align: center;
    font-weight: bold;
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
  