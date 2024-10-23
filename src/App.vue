<template>
  <main
    class="flex flex-col items-center p-4 md:space-x-6 space-y-4 md:p-4 md:flex-row md:items-start lg:p-0"
  >
    <ProductDisplay :products="products" />
    <Cart :cart="cart" @open-confirmation="openConfirmation" />
  </main>
  <OrderConfirmation
    :is-open="isConfirmationOpen"
    :cart="cart"
    @close-confirmation="closeConfirmation"
  />
</template>

<script setup>
import { ref } from 'vue'
import Cart from './components/Cart.vue'
import ProductDisplay from './components/ProductDisplay.vue'
import OrderConfirmation from './components/OrderConfirmation.vue'

import productData from '../data.json'
import { provide } from 'vue'

const products = productData
const cart = ref([])

const isConfirmationOpen = ref(false)

const openConfirmation = () => (isConfirmationOpen.value = true)
const closeConfirmation = () => {
  isConfirmationOpen.value = false
  cart.value = []
}

const findItemInCart = product =>
  cart.value.find(item => item.id === product.id)

const addToCart = product => {
  const itemInCart = findItemInCart(product)
  if (!itemInCart) {
    cart.value.push({ ...product, quantity: 1 })
  } else {
    itemInCart.quantity++
  }
}

const decrement = product => {
  const itemInCart = findItemInCart(product)
  if (itemInCart && itemInCart.quantity > 1) {
    itemInCart.quantity--
  }
}

const inCart = product => !!findItemInCart(product)

const showQuantityInCart = product => {
  const itemInCart = findItemInCart(product)
  return itemInCart ? itemInCart.quantity : 0
}

const removeFromCart = productId => {
  cart.value = cart.value.filter(item => item.id !== productId)
}

function formatarNumero(numero) {
  return numero.toFixed(2)
}

const showTotal = () => {
  let total = 0
  cart.value.forEach(item => {
    total += item.price * item.quantity
  })

  return formatarNumero(total)
}

provide('storeMethods', {
  formatarNumero,
  addToCart,
  inCart,
  decrement,
  showQuantityInCart,
  showTotal,
  removeFromCart,
})
</script>
