<template>
    <div>
        123
    </div>
    <!-- не вошёл -->
    <div class="w-full flex flex-col gap-6 items-center text-center py-8 px-4 bg-[#2C2C2C]/90 rounded-xl h-fit">
        <img src="/images/index/main.jpg" alt="" class="rounded-xl w-full md:w-2/3 lg:w-1/2">
        <p class="mainHeading">Исследуй технологии</p>
        <p class="font-light">Создавай выставки и сохраняй лучшие экспонаты</p>
        <NuxtLink to="/" class="bg-white text-[#2C2C2C] transition-all duration-500 border border-transparent hover:bg-transparent hover:text-white hover:border-white hover:-translate-y-1 shadow-md px-4 py-1.5 rounded-xl">Присоединиться</NuxtLink>
    </div>
    <!-- вошёл -->
    <div class="w-full flex flex-col gap-6 items-center text-center py-8 px-4 bg-[#2C2C2C]/90 rounded-xl h-fit">
        <img src="/images/index/main.jpg" alt="" class="rounded-xl w-full md:w-2/3 lg:w-1/2">
        <p class="mainHeading">Добро пожаловать!</p>
        <p class="font-light">Создавай выставки и сохраняй лучшие экспонаты</p>
        <NuxtLink to="/" class="bg-white text-[#2C2C2C] transition-all duration-500 border border-transparent hover:bg-transparent hover:text-white hover:border-white hover:-translate-y-1 shadow-md px-4 py-1.5 rounded-xl">Начать</NuxtLink>
    </div>
</template>

<script setup>

</script>