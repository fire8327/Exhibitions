<template>
      <div class="flex flex-col gap-6">
        <p class="mainHeading">Все выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
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
  }
]
</script>