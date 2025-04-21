<template>
    <div class="grow flex items-center justify-center">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 w-full md:w-2/3">
            <NuxtLink v-for="category in categories" :key="category.slug" to="/"
                class="bg-[#2C2C2C]/90 shadow-md rounded-lg p-4 flex items-center flex-col gap-4 hover:shadow-lg transition-all duration-300 hover:bg-[#2C2C2C] h-fit">
                <Icon class="text-3xl text-cyan-500" :name="category.icon" />
                <span class="text-base font-medium">{{ category.name }}</span>
            </NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* категории */
const categories = [
  {
    slug: 'smartphones',
    name: 'Смартфоны',
    icon: 'mdi:cellphone',
  },
  {
    slug: 'laptops',
    name: 'Ноутбуки',
    icon: 'mdi:laptop',
  },
  {
    slug: 'gadgets',
    name: 'Гаджеты',
    icon: 'mdi:watch',
  },
  {
    slug: 'audio',
    name: 'Аудио',
    icon: 'mdi:headphones',
  },
  {
    slug: 'gadgets',
    name: 'Гаджеты',
    icon: 'mdi:watch',
  },
  {
    slug: 'audio',
    name: 'Аудио',
    icon: 'mdi:headphones',
  },
]
</script>