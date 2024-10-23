<template>
  <div class="w-full mx-auto bg-white rounded-lg p-4 md:w-[350px]">
    <h2 class="font-bold text-red text-xl">Your cart ({{ cart.length }})</h2>
    <div
      v-if="!cart.length"
      class="flex flex-col items-center justify-center p-4"
    >
      <img src="/images/illustration-empty-cart.svg" alt="Empty cart" />
      <p class="text-sm text-rose-800 mt-2">
        Your added items will appear here
      </p>
    </div>
    <div v-else>
      <div class="mt-3">
        <ul>
          <li
            class="flex justify-between items-center border-b border-rose-100 py-1.5"
            v-for="(product, index) in cart"
            :key="index"
          >
            <div class="space-y-0.5">
              <p class="font-bold">{{ product.name }}</p>
              <p class="space-x-3 text-red">
                <strong class="text-red">{{ product.quantity }}x</strong>
                <span
                  ><span class="text-sm">@</span> ${{
                    formatarNumero(product.price)
                  }}</span
                >
                <span class="text-rose-800 font-semibold"
                  >${{ formatarNumero(product.price * product.quantity) }}</span
                >
              </p>
            </div>
            <button
              @click.prevent="removeFromCart(product.id)"
              class="text-rose-500 w-5 h-5 rounded-full border-2 border-rose-500 font-semibold flex items-center justify-center hover:text-black hover:border-black"
            >
              <IconRemove />
            </button>
          </li>
        </ul>
      </div>
      <div class="pt-4 space-y-6">
        <div class="flex items-center justify-between">
          <p>Order total</p>
          <p class="font-bold text-lg">${{ showTotal() }}</p>
        </div>

        <p
          class="bg-rose-50 p-2 py-3 text-center rounded-md flex items-center justify-center gap-2"
        >
          <IconCarbonNeutral /> This is <strong>carbon-neutral</strong> delivery
        </p>

        <button
          @click.prevent="emit('open-confirmation')"
          class="p-4 rounded-full w-full bg-red text-white capitalize font-semibold"
        >
          confirm order
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { inject } from 'vue'
import IconRemove from './icons/IconRemove.vue'
import IconCarbonNeutral from './icons/IconCarbonNeutral.vue'
defineProps({
  cart: {
    type: Array,
    required: true,
  },
})

const { formatarNumero, removeFromCart, showTotal } = inject('storeMethods')
const emit = defineEmits(['open-confirmation'])
//const totalCart = 0 open-confirmation
</script>
