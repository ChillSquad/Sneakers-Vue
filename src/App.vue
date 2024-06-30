<script setup>
import { ref, provide, onMounted, watch, computed } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'

import MyHeader from './components/MyHeader.vue'
import CardList from './components/CardList.vue'
import MyDrawer from './components/MyDrawer.vue'

const openDrawer = ref(false)

const drawerClose = () => {
  openDrawer.value = false
}

const drawerOpen = () => {
  openDrawer.value = true
}

const items = ref([])
const cart = ref([])

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))

const filters = ref({
  sortBy: 'title',
  searchQuery: ''
})

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const createOrder = async () => {
  try {
    const { data } = await axios.post('https://d97b674d785669b8.mokky.dev/orders', {
      items: cart.value,
      totalPrice: totalPrice.value
    })
    cart.value = []

    return data
  } catch (error) {
    console.log(error)
  }
}

const onChangeSelect = (event) => {
  filters.value.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.value.searchQuery = event.target.value
}, 500)

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://d97b674d785669b8.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      const { data } = await axios.post('https://d97b674d785669b8.mokky.dev/favorites', obj)

      item.isFavorite = true
      item.favoriteId = data.id
    } else {
      await axios.delete(`https://d97b674d785669b8.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.value.sortBy
    }

    if (filters.value.searchQuery) {
      params.title = `*${filters.value.searchQuery}*`
    }

    const { data } = await axios.get(`https://d97b674d785669b8.mokky.dev/items`, {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems, { deep: true })

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)
provide('cartActions', { cart, drawerClose, drawerOpen, addToCart, removeFromCart })
</script>

<template>
  <MyDrawer v-if="openDrawer" @create-order="createOrder" :total-price="totalPrice" />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <MyHeader :total-price="totalPrice" @drawer-open="drawerOpen" />

    <section class="p-10">
      <div class="flex justify-between">
        <h1 class="text-3xl font-bold select-none">Все кроссовки</h1>

        <div class="flex gap-4">
          <select
            @change="onChangeSelect"
            class="border rounded-md pl-2 pr-2 outline-none focus:border-gray-400 cursor-pointer"
          >
            <option value="title">По названию</option>
            <option value="price">Дешевле</option>
            <option value="-price">Дороже</option>
          </select>

          <div class="relative">
            <img
              src="../public/search.svg"
              alt="search"
              class="absolute left-1.5 top-0.5 p-2 pointer-events-none"
            />
            <input
              @input="onChangeSearchInput"
              type="text"
              placeholder="Поиск..."
              class="border rounded-md pl-10 pr-2 outline-none focus:border-gray-400 h-full"
            />
          </div>
        </div>
      </div>
      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </section>
  </div>
</template>
