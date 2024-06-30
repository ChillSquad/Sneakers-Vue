<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cart, removeFromCart } = inject('cartActions')
</script>

<template>
  <section v-auto-animate class="grid gap-4 mt-8 max-w-sm">
    <CartItem
      v-for="item in cart"
      v-bind:key="item.id"
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </section>
</template>
