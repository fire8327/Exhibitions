<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">О нас</p>
        <div class="flex max-lg:flex-col gap-6 w-full">
            <div class="flex flex-col gap-4 w-full lg:w-1/2 font-light">
                <p>
                    <span class="font-medium text-white">Мы</span> — цифровая площадка, где можно познакомиться с последними технологическими новинками и трендами.
                    Наша платформа объединяет информацию о современных гаджетах, от смартфонов и ноутбуков до умной
                    домашней техники и носимых устройств.
                </p>
                <p>
                    <span class="font-medium text-white">Здесь</span> вы найдёте подробные описания устройств, их характеристики и возможности. Мы стремимся создать
                    удобный каталог, который поможет разобраться в многообразии технологий и сделать осознанный выбор.
                </p>
                <p>
                    <span class="font-medium text-white">Наша цель</span> — просто и понятно рассказывать о сложных технологиях. Мы собираем информацию о новых
                    продуктах, анализируем их особенности и представляем в удобном формате. Это место, где можно узнать,
                    что действительно важно в современных устройствах.
                </p>
            </div>
            <img src="/images/about/main.jpg" alt="" class="w-full lg:w-1/2 rounded-xl aspect-video object-cover">
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Наша команда</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
            <div v-for="member in team" :key="member.name"
                class="bg-gradient-to-b from-[#2C2C2C]/90 to-[#1A1A1A] rounded-2xl overflow-hidden shadow-lg text-center transition-all duration-500 hover:shadow-xl">
                <img :src="member.image" alt="" class="aspect-[7/8] object-cover">
                <div class="flex flex-col gap-2 p-6">
                    <p class="text-lg font-semibold text-white">{{ member.name }}</p>
                    <p class="font-light">{{ member.role }}</p>
                </div>
            </div>
        </div>
    </div>
    <div class="relative flex flex-col gap-6">
        <iframe class="w-full h-[400px] rounded-xl border border-gray-300 shadow-md lg:h-[500px]"
            src="https://yandex.ru/map-widget/v1/?um=constructor%3Ab5b71122d59bf321fa1fb00171146b95ba02bcfc87108b9fa87ff29b94af771d&source=constructor"
            frameborder="0"></iframe>
        <div
            class="flex flex-col gap-4 lg:w-3/4 lg:mx-auto lg:-mt-16 lg:bg-[#2C2C2C] lg:p-6 lg:rounded-xl lg:shadow-lg">
            <p class="mainHeading">Контакты</p>
            <div class="flex flex-col lg:flex-row lg:flex-wrap gap-4 lg:gap-6">
                <div class="flex flex-col gap-2">
                    <p class="text-base">Адрес</p>
                    <p class="text-xl font-semibold font-mono text-white">улица Пушкина, дом 1</p>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Режим работы</p>
                    <p class="mt-2 text-xl font-semibold font-mono text-white">Пн-Пт: с 9:00 до 21:00</p>
                    <p class="text-xl font-semibold font-mono text-white">Cб-Вс: с 12:00 до 18:00</p>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Email</p>
                    <NuxtLink to="mailto:exhibitions@mail.ru"
                        class="text-xl font-semibold font-mono text-white transition-all duration-500 hover:text-sky-500">
                        exhibitions@mail.ru</NuxtLink>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Телефон для связи</p>
                    <NuxtLink to="tel:8 800-555-35-35"
                        class="text-xl font-semibold font-mono text-white transition-all duration-500 hover:text-sky-500">
                        8 800-555-35-35</NuxtLink>
                </div>
                <div class="flex items-center gap-2">
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-sky-500">
                        <Icon class="text-3xl" name="mdi:vk" />
                    </NuxtLink>
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-sky-500">
                        <Icon class="text-3xl" name="ic:baseline-telegram" />
                    </NuxtLink>
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-sky-500">
                        <Icon class="text-3xl" name="basil:instagram-outline" />
                    </NuxtLink>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const team = [
  {
    name: 'Алексей Ковалёв',
    role: 'Основатель и CEO',
    image: '/images/about/2.jpg'
  },
  {
    name: 'Мария Соколова',
    role: 'Дизайнер интерфейсов',
    image: '/images/about/1.jpg'
  },
  {
    name: 'Дмитрий Иванов',
    role: 'Ведущий разработчик',
    image: '/images/about/3.jpg'
  }
]
</script>