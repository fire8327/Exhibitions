<template>
    <Header/>
    <Loader v-if="isLoading"/> 
    <main class="grid-container gap-y-14 md:gap-y-20 xl:gap-y-28 grow my-10">
        <slot/>        
    </main>
   <!--  <Footer/> -->
</template>

<script setup>
const isLoading = ref(true)

const nuxtApp = useNuxtApp()

// хук для начала загрузки страницы
nuxtApp.hook('page:start', () => {
  isLoading.value = true
})

// хук для завершения загрузки страницы
nuxtApp.hook('page:finish', () => {
  setTimeout(() => {
    isLoading.value = false
  }, 300) // задержка для надёжности
})

// обработка ошибок загрузки
nuxtApp.hook('app:error', () => {
  isLoading.value = false
})
</script>

<style scoped>
.animate-fade-in { animation: fadeIn 0.5s ease-out; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
</style>