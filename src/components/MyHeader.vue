<script setup>
const emit = defineEmits(['drawerOpen'])

defineProps({
  totalPrice: Number
})
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <div class="flex items-center">
      <a href="index.html">
        <img src="/logo.png" alt="logo" class="w-14" />
      </a>

      <div class="ml-4 select-none">
        <h2 class="uppercase font-bold text-xl">Vue sneakers</h2>
        <p class="text-slate-400 text-sm">Магазин лучших кросовок</p>
      </div>
    </div>

    <ul class="flex flex-row items-center text-gray-500">
      <li @click="() => emit('drawerOpen')" class="flex gap-2 hover:text-black cursor-pointer">
        <img src="/cart.svg" alt="cart" />
        <b>{{ totalPrice }} руб.</b>
      </li>

      <!-- <li class="flex gap-2 ml-10 hover:text-black cursor-pointer">
        <img src="/heart.svg" alt="heart" />
        <p>Закладки</p>
      </li>

      <li class="flex gap-2 ml-10 hover:text-black cursor-pointer">
        <img src="/profile.svg" alt="profile" />
        <p>Профиль</p>
      </li> -->
    </ul>
  </header>
</template>
