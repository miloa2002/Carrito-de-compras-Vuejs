<script setup>
import { computed, ref } from 'vue';
import Product from './components/Product.vue';
import ProductSummary from './components/ProductSummary.vue'
import { data } from './data/data';

const products = ref(data)
const cart = ref([])

const addToCart = (product) => {
    product.quantity = 1
    cart.value.push(product)
}

const totalProducts = computed(() => {
  return cart.value.reduce((acum, product) => acum + (product.quantity * product.price), 0)
})

const deleteProduct = (id) => {
  cart.value = cart.value.filter(e => e.id !== id)
}

const increaseQuantity = (id) => {
  const searchProduct = cart.value.findIndex(product => product.id === id)
  const maxItems = cart.value[searchProduct].quantity
  if(maxItems === 5) {
    cart.value[searchProduct].quantity = 0
  }else{ 
    cart.value[searchProduct].quantity++
  }
}

const decreaseQuantity = (id) => {
  const searchProduct = cart.value.findIndex(product => product.id === id)
  const minItems = cart.value[searchProduct].quantity
  if(minItems === 0) {
    cart.value[searchProduct].quantity = 0
  }else{ 
    cart.value[searchProduct].quantity--
  }
}

</script>

<template>
  <main class="container mx-auto mb-14">
    <div class="md:flex items-start gap-8">
      <div class="w-full sm:w-2/3">
        <h1 class="text-6xl font-bold mb-5">Postres</h1>
        <div class="grid grid-cols-3 items-center gap-4">
          <Product v-for="product in products" :key="product.id" :product="product" :cart="cart" @add-to-cart="addToCart" @increase-quantity="increaseQuantity" @decrease-quantity="decreaseQuantity" />
        </div>
      </div>

      <div class="w-full sm:w-1/3 h-auto bg-white p-8 rounded-lg mt-10">
        <div v-if="cart.length <= 0">
          <p class="text-2xl text-[#BC3C12] font-bold mb-8">Tu carrito <span>(0)</span></p>
          <div class="text-center">
            <img class="mx-auto" src="/images/illustration-empty-cart.svg" alt="No hay productos">
            <p class="text-[#8A7A78] font-medium">Los artículos que agregues aparecerán aquí</p>
          </div>
        </div>

        <div v-else>
          <p class="text-2xl text-[#BC3C12] font-bold mb-8">Tu carrito <span>({{ cart.length }})</span></p>

          <ProductSummary v-for="product in cart" :product="product" @delete-product="deleteProduct" />

          <div class="mt-8 flex items-center justify-between">
            <p class="text-[#BC3C12] font-bold">Total:</p>
            <p class="text-[#6e615f] font-extrabold text-3xl">${{ totalProducts }}</p>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>