<script setup>
defineProps({
  imageURL: String,
  title: String,
  description: String
})
</script>

<template>
  <div class="flex flex-col justify-center text-center max-w-64 mx-auto items-center">
    <img :src="imageURL" alt="Info image" class="size-32" />
    <h2 class="text-2xl font-medium">{{ title }}</h2>
    <p class="text-gray-400 mt-2 max-w-64">{{ description }}</p>
  </div>
</template>
