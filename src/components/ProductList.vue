<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">Product List</h1>
    <ul>
      <li v-for="product in products" :key="product._id" class="mb-2">
        <div class="p-4 bg-white shadow rounded">
          <h2 class="text-xl">{{ product.name }}</h2>
          <p>Price: {{ product.price }}€</p>
          <p v-if="product.promotion">Promotion: {{ product.discount }}% off</p>
          <p>Category: {{ product.category.name }}</p>
        </div>
      </li>
    </ul>
    <router-link to="/add-product">
      <button class="mt-4 p-2 bg-blue-500 text-white rounded">Add Product</button>
    </router-link>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { API_URL } from '../constants';

const products = ref([]);

const fetchProducts = async () => {
  const response = await fetch(API_URL);
  products.value = await response.json();
};

onMounted(fetchProducts);
</script>
