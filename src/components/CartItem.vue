<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})
</script>

<template>
  <div class="flex bg-white border border-slate-200 rounded-xl p-4 transition items-center h-32">
    <img class="w-16 h-16 mr-4" :src="imageUrl" :alt="title" />

    <div class="flex flex-col justify-between w-full">
      <p class="mt-1">{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b class="">{{ price }} руб.</b>
        <img
          @click="emit('onClickRemove')"
          src="/public/close.svg"
          alt="plus"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
        />
      </div>
    </div>
  </div>
</template>
