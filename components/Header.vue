<template>
    <header class="grid-container py-4 text-lg font-medium relative">
        <!-- Основной контейнер -->
        <div
            class="w-full flex items-center gap-4 justify-between px-4 py-2 bg-[#2C2C2C]/90 rounded-xl shadow-[0px_0px_20px_-13px_black]">
            <!-- Логотип -->
            <NuxtLink to="/" class="flex items-center gap-2 text-cyan-500 transition-all duration-500 hover:opacity-70">
                <Icon class="text-3xl" name="streamline:ai-technology-spark-solid" />
                <span class="text-xl font-mono font-semibold">Exhibitions</span>
            </NuxtLink>

            <!-- Бургер-иконка для мобильных устройств -->
            <button @click="isMenuShow = !isMenuShow" class="lg:hidden flex cursor-pointer">
                <Icon class="text-3xl text-cyan-500" :name="isMenuShow ? 'material-symbols:close' : 'ic:round-menu'" />
            </button>

            <!-- Меню (единое для десктопа и мобильного) -->
            <div class="lg:flex lg:items-center lg:gap-6 text-white max-lg:fixed static bottom-0 left-0 w-full bg-[#2C2C2C] lg:bg-transparent rounded-t-xl max-lg:shadow-[0px_0px_20px_-13px_black] z-[5] transition-all duration-500"
                :class="{ 'translate-y-0': isMenuShow, 'translate-y-full lg:translate-y-0': !isMenuShow }">
                <div class="flex flex-col lg:flex-row gap-4 p-4 lg:p-0">
                    <NuxtLink to="/" class="mainLink px-4 py-1 rounded-xl transition-all duration-500 lg:bg-[#3C3C3C] lg:shadow-md hover:bg-[#000000]/15 hover:border-cyan-500 border border-transparent">
                        Главная
                    </NuxtLink>
                    <NuxtLink to="/about" class="mainLink px-4 py-1 rounded-xl transition-all duration-500 lg:bg-[#3C3C3C] lg:shadow-md hover:bg-[#000000]/15 hover:border-cyan-500 border border-transparent">
                        О нас
                    </NuxtLink>
                    <NuxtLink to="/exhibitions" class="mainLink px-4 py-1 rounded-xl transition-all duration-500 lg:bg-[#3C3C3C] lg:shadow-md hover:bg-[#000000]/15 hover:border-cyan-500 border border-transparent">
                        Выставки
                    </NuxtLink>
                    <NuxtLink to="/categories" class="mainLink px-4 py-1 rounded-xl transition-all duration-500 lg:bg-[#3C3C3C] lg:shadow-md hover:bg-[#000000]/15 hover:border-cyan-500 border border-transparent">
                        Категории
                    </NuxtLink>
                    <!-- Иконка профиля (всегда видна на мобилке) -->
                    <NuxtLink to="/auth" class="lg:hidden flex transition-all duration-500 hover:opacity-70">
                        <Icon class="text-3xl text-cyan-500" name="material-symbols:person" />
                    </NuxtLink>
                </div>
            </div>

            <!-- Иконка профиля (всегда видна на десктопе) -->
            <NuxtLink to="/auth" class="hidden lg:flex transition-all duration-500 hover:opacity-70">
                <Icon class="text-3xl text-cyan-500" name="material-symbols:person" />
            </NuxtLink>
        </div>

        <!-- Оверлей для мобильного меню -->
        <div @click="isMenuShow = false" class="lg:hidden fixed z-[4] inset-0 bg-black/30 transition-all duration-500"
            :class="{ 'opacity-0 pointer-events-none': !isMenuShow, 'opacity-100': isMenuShow }"></div>

        <!-- Уведомление (если есть) -->
        <!-- <button type="button" @click="messageTitle = null"
            class="lg:fixed lg:top-10 lg:right-4 max-lg:sticky max-lg:top-4 max-lg:mx-auto z-[11] cursor-pointer flex items-center gap-2 px-4 py-1 text-base rounded-xl w-fit font-medium font-mono border-white border bg-[#2C2C2C] shadow-[0px_0px_13px_-7px_black]"
            :class="messageType ? 'text-white' : 'text-red-500'" v-if="messageTitle">
            <Icon class="text-2xl" name="material-symbols:close-small-rounded" />
            <span>{{ messageTitle }}</span>
        </button> -->
    </header>
</template>

<script setup>
/* открытие мобильного меню */
const isMenuShow = ref(false) 


/* закрытие мобильного меню */
const nuxtApp = useNuxtApp()
nuxtApp.hook('page:start', () => {
    isMenuShow.value = false
})
</script>