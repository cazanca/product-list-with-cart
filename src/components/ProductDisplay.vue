<template>
  <section class="min-w-full flex-1 md:min-w-[50%]">
    <h1 class="font-bold text-3xl">Desserts</h1>
    <div id="cards" class="mt-4">
      <div v-for="(product, index) in products" :key="index" class="">
        <div class="rounded">
          <img
            class="rounded-lg object-cover"
            :src="product.image.desktop"
            :alt="product.name"
          />
        </div>
        <div class="flex justify-center -mt-4">
          <button
            v-if="!inCart(product)"
            @click="addToCart(product)"
            class="border border-red font-semibold text-sm flex items-center gap-2 bg-white rounded-full px-6 py-2 hover:text-red"
          >
            <IconCart class="w-4" />
            <span>Add to cart</span>
          </button>

          <div
            v-else
            class="bg-red font-semibold text-sm flex items-center gap-8 rounded-full px-4 py-2"
          >
            <button
              @click.prevent="decrement(product)"
              class="border border-white w-5 h-5 rounded-full flex items-center justify-center cursor-pointer text-white hover:bg-white hover:text-red"
            >
              <IconDecrement />
            </button>

            <span class="text-white font-semibold">{{ showQuantityInCart(product) }}</span>

            <button
              @click.prevent="addToCart(product)"
              class="border border-white w-5 h-5 rounded-full flex items-center justify-center cursor-pointer text-white hover:bg-white hover:text-red"
            >
              <IconIncrement />
            </button>
          </div>
        </div>
        <div class="space-y-0.5 mt-2.5">
          <span class="text-rose-500">{{ product.category }}</span>
          <p class="font-bold">{{ product.name }}</p>
          <p class="text-red font-bold">${{ formatarNumero(product.price) }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { inject } from 'vue'
import IconCart from './icons/IconCart.vue'
import IconDecrement from './icons/IconDecrement.vue'
import IconIncrement from './icons/IconIncrement.vue'

defineProps({
  products: {
    type: Array,
    required: true,
  },
})

const { formatarNumero, addToCart, inCart, decrement, showQuantityInCart } =
  inject('storeMethods')
</script>
