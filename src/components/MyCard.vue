<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function,
  onClickAdd: Function
})
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      @click="onClickFavorite"
      :src="!isFavorite ? './like-1.svg' : './like-2.svg'"
      alt="like"
      class="absolute top-10 left-10 rounded-md transition hover:drop-shadow-md"
    />

    <img :src="imageUrl" alt="sneakers" />

    <p class="mt-1">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} руб.</b>
      </div>

      <div>
        <img
          @click="onClickAdd"
          :src="!isAdded ? './plus.svg' : './checked.svg'"
          alt="plus"
          class="rounded-md transition hover:drop-shadow-md"
        />
      </div>
    </div>
  </div>
</template>
