<template>
  <teleport to="body">
    <div
      v-if="isOpen"
      class="fixed inset-0 bg-black bg-opacity-75 h-screen flex items-center justify-center"
    >
      <div class="bg-white min-w-[370px] rounded-lg p-4">
        <IconConfirmed class="text-green w-10" />
        <p class="text-3xl mt-2"><strong>Order Confirmed</strong></p>
        <p class="text-sm text-rose-500 mt-1">We hope you enjoy you food!</p>

        <ul class="bg-rose-50 p-2 mt-2 rounded">
          <li
            class="flex justify-between items-center border-b border-rose-100 py-1.5"
            v-for="(product, index) in cart"
            :key="index"
          >
            <div class="flex items-center gap-2 text-sm">
              <div class="w-12 h-12">
                <img :src="product.image.thumbnail" :alt="product.name" />
              </div>
              <div class="space-y-0.5">
                <p class="font-semibold">{{ product.name }}</p>
                <p class="space-x-3 text-red">
                  <strong class="text-red">{{ product.quantity }}x</strong>
                  <span
                    ><span class="text-sm">@</span> ${{
                      formatarNumero(product.price)
                    }}</span
                  >
                </p>
              </div>
            </div>
            <p class="text-sm font-semibold">
              ${{ formatarNumero(product.price * product.quantity) }}
            </p>
          </li>
          <li class="flex items-center justify-between py-2 mt-2">
            <p class="text-sm">Order total</p>
            <p class="font-bold text-lg">${{ showTotal() }}</p>
          </li>
        </ul>

        <button
          @click.prevent="emit('close-confirmation')"
          class="p-4 rounded-full w-full bg-red text-white capitalize font-semibold"
        >
          start new order
        </button>
      </div>
    </div>
  </teleport>
</template>

<script setup>
import { inject } from 'vue'
import IconConfirmed from './icons/IconConfirmed.vue'

defineProps({
  isOpen: {
    type: Boolean,
    required: true,
  },
  cart: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits(['close-confirmation'])

const { formatarNumero, showTotal } = inject('storeMethods')
</script>
