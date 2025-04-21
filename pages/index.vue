<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Текущие выставки</p>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <NuxtLink to="/" v-for="exhibition in exhibitions"
                class="group bg-gradient-to-b from-[#2C2C2C]/90 to-[#1C1C1C] rounded-2xl overflow-hidden transition-all duration-500 shadow-xl">
                <div class="relative overflow-hidden">
                    <img :src="exhibition.image" :alt="exhibition.title"
                        class="w-full aspect-video object-cover brightness-75 transition-all duration-500 group-hover:brightness-100 group-hover:scale-105" />
                </div>
                <div class="p-6 flex flex-col gap-2">
                    <span class="mainHeading">{{ exhibition.title }}</span>
                    <p class="font-light line-clamp-2">
                        {{ exhibition.description }}
                    </p>
                    <span
                        class="flex bg-gradient-to-r from-cyan-500 to-cyan-600 text-white px-4 py-1.5 rounded-full font-medium transition-all duration-500 group-hover:-translate-y-1 group-hover:shadow-md w-fit">
                        Подробнее
                    </span>
                </div>
            </NuxtLink>
        </div>
    </div>
    <!-- не вошёл -->
    <div class="bgStars w-full flex max-lg:flex-col gap-6 items-center text-center py-8 px-4 bg-[#2C2C2C]/90 rounded-2xl h-fit shadow-xl">
        <img src="/images/index/main.jpg" alt="" class="rounded-xl w-full lg:w-1/2">
        <div class="flex flex-col gap-2 w-full lg:w-1/2 items-center text-center">
            <p class="mainHeading">Исследуй технологии</p>
            <p class="font-light">Создавай выставки и сохраняй лучшие экспонаты</p>
            <NuxtLink to="/"
                class="bg-gradient-to-r from-cyan-500 to-cyan-600 w-52 text-white transition-all duration-500 hover:-translate-y-1 hover:shadow-lg shadow-md px-4 py-1.5 rounded-full">
                Присоединиться</NuxtLink>
        </div>
    </div>
    <!-- вошёл -->
    <div class="bgStars w-full flex max-lg:flex-col gap-6 items-center text-center py-8 px-4 bg-[#2C2C2C]/90 rounded-2xl h-fit shadow-xl">
        <img src="/images/index/main.jpg" alt="" class="rounded-xl w-full lg:w-1/2">
        <div class="flex flex-col gap-2 w-full lg:w-1/2 items-center text-center">
            <p class="mainHeading">Добро пожаловать!</p>
            <p class="font-light">Создавай выставки и сохраняй лучшие экспонаты</p>
            <NuxtLink to="/"
                class="bg-gradient-to-r from-cyan-500 to-cyan-600 w-52 text-white transition-all duration-500 hover:-translate-y-1 hover:shadow-lg shadow-md px-4 py-1.5 rounded-full">
                Начать</NuxtLink>
        </div>
    </div>
</template>

<script setup>
// Рандомные данные для выставок
const exhibitions = [
  {
    id: '1',
    title: 'Tech Expo 2025',
    description: 'Новейшие смартфоны, ноутбуки и умные гаджеты от ведущих брендов.',
    image: 'https://img.freepik.com/free-photo/person-using-ar-technology-their-daily-occupation_23-2151137526.jpg?t=st=1745230247~exp=1745233847~hmac=1cb656314088a7141ba658d02befb10c1c3e278a6bf1bf7cc675dbc137bf7bc9&w=996',
    slug: 'tech-expo-2025',
  },
  {
    id: '2',
    title: 'Gadget Showcase',
    description: 'Инновационные устройства для дома и работы, представленные в AR.',
    image: 'https://img.freepik.com/free-photo/person-using-ar-technology-their-daily-occupation_23-2151137526.jpg?t=st=1745230247~exp=1745233847~hmac=1cb656314088a7141ba658d02befb10c1c3e278a6bf1bf7cc675dbc137bf7bc9&w=996',
    slug: 'gadget-showcase',
  },
]

// Рандомные данные для категорий
const categories = [
  {
    slug: 'smartphones',
    name: 'Смартфоны',
    icon: 'https://via.placeholder.com/48?text=📱',
  },
  {
    slug: 'laptops',
    name: 'Ноутбуки',
    icon: 'https://via.placeholder.com/48?text=💻',
  },
  {
    slug: 'gadgets',
    name: 'Гаджеты',
    icon: 'https://via.placeholder.com/48?text=⌚',
  },
  {
    slug: 'audio',
    name: 'Аудио',
    icon: 'https://via.placeholder.com/48?text=🎧',
  },
]
</script>