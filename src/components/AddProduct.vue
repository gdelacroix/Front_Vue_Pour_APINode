<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">Add Product</h1>
    <form @submit.prevent="addProduct">
      <div class="mb-4">
        <label class="block text-sm font-bold mb-2">Name</label>
        <input v-model="name" class="border rounded w-full p-2" />
      </div>
      <div class="mb-4">
        <label class="block text-sm font-bold mb-2">Price</label>
        <input type="number" v-model="price" class="border rounded w-full p-2" />
      </div>
      <div class="mb-4">
        <label class="block text-sm font-bold mb-2">Promotion</label>
        <input type="checkbox" v-model="promotion" />
      </div>
      <div class="mb-4">
        <label class="block text-sm font-bold mb-2">Discount</label>
        <input type="number" v-model="discount" class="border rounded w-full p-2" />
      </div>
      <div class="mb-4">
        <label class="block text-sm font-bold mb-2">Category</label>
        <select v-model="selectedCategory" class="border rounded w-full p-2">
          <option v-for="category in categories" :key="category" :value="category">
            {{ category }}
          </option>
          <option value="new">Create New Category</option>
        </select>
      </div>
      <div v-if="selectedCategory === 'new'" class="mb-4">
        <label class="block text-sm font-bold mb-2">New Category Name</label>
        <input v-model="newCategoryName" class="border rounded w-full p-2" />
      </div>
      <button type="submit" class="p-2 bg-green-500 text-white rounded">Submit</button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { API_URL } from '../constants';

const name = ref('');
const price = ref(0);
const promotion = ref(false);
const discount = ref(0);
const selectedCategory = ref('');
const newCategoryName = ref('');
const categories = ref<string[]>([]);

const fetchProducts = async () => {
  const response = await fetch(API_URL);
  const products = await response.json();
  
  // Extract unique categories from the products
  categories.value = Array.from(new Set(products.map((product: any) => product.category.name)));
};

onMounted(fetchProducts);

const addProduct = async () => {
  const category = selectedCategory.value === 'new' ? newCategoryName.value : selectedCategory.value;

  const response = await fetch(`${API_URL}/add`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      name: name.value,
      price: price.value,
      promotion: promotion.value,
      discount: discount.value,
      category: {
        name: category,
      },
    }),
  });

  if (response.ok) {
    alert('Product added successfully');
  } else {
    alert('Failed to add product');
  }
};
</script>
