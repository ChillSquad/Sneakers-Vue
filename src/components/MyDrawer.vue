<script setup>
import { inject, onMounted, onUnmounted } from 'vue'

import CartItemList from './CartItemList.vue'
import MyDrawerHead from './MyDrawerHead.vue'
import InfoBlock from './InfoBlock.vue'

const { drawerClose } = inject('cartActions')

const emit = defineEmits(['createOrder'])

defineProps({
  totalPrice: Number
})

const handleKeyDown = (event) => {
  if (event.key === 'Escape') {
    drawerClose()
  }
}

onMounted(() => {
  window.addEventListener('keydown', handleKeyDown)
})

onUnmounted(() => {
  window.removeEventListener('keydown', handleKeyDown)
})
</script>

<template>
  <div @click="drawerClose" class="fixed top-0 left-0 bg-black w-full h-full opacity-70 z-10"></div>

  <aside class="fixed top-0 right-0 bg-white min-w-96 h-full z-20 p-8 flex flex-col">
    <div class="flex items-center gap-5">
      <MyDrawerHead />
    </div>

    <div v-if="!totalPrice" class="flex h-full">
      <InfoBlock
        imageURL="./package-icon.png"
        title="Корзина пустая"
        description="Добавте хотя бы одну пару кросовок, чтобы сделать заказ"
      />
    </div>

    <div v-else class="flex-grow overflow-y-auto">
      <CartItemList />

      <section class="grid gap-3 my-5">
        <div class="flex">
          <span>Итого:</span>
          <div class="flex-1 border-b-2 border-dotted ml-2 mr-2 mb-1"></div>
          <b>{{ totalPrice }} руб.</b>
        </div>

        <button
          :disabled="totalPrice ? false : true"
          @click="() => emit('createOrder')"
          class="mt-4 bg-lime-500 w-full rounded-xl py-3 min-w-96 text-white hover:bg-lime-600 active:bg-lime-700 transition cursor-pointer disabled:bg-slate-300 disabled:cursor-default"
        >
          Оформить заказ
        </button>
      </section>
    </div>
  </aside>
</template>
