<template>
    <!-- статистика -->
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Наша платформа</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="bg-[#2C2C2C]/90 rounded-xl p-5 text-center shadow-md transition-all duration-500 hover:bg-[#2C2C2C] hover:shadow-lg group">
                <p class="text-2xl font-bold text-white mb-1">{{ exhibitionsStats ? exhibitionsStats.length : '0' }}</p>
                <p class="font-light">Выставок</p>
            </div>
            <div class="bg-[#2C2C2C]/90 rounded-xl p-5 text-center shadow-md transition-all duration-500 hover:bg-[#2C2C2C] hover:shadow-lg group">
                <p class="text-2xl font-bold text-white mb-1">{{ devices ? devices.length : '0' }}</p>
                <p class="font-light">Экспонатов</p>
            </div>
            <div class="bg-[#2C2C2C]/90 rounded-xl p-5 text-center shadow-md transition-all duration-500 hover:bg-[#2C2C2C] hover:shadow-lg group">
                <p class="text-2xl font-bold text-white mb-1">{{ users ? users.length : '0' }}</p>
                <p class="font-light">Пользователей</p>
            </div>
            <div class="bg-[#2C2C2C]/90 rounded-xl p-5 text-center shadow-md transition-all duration-500 hover:bg-[#2C2C2C] hover:shadow-lg group">
                <p class="text-2xl font-bold text-white mb-1">{{ categories ? categories.length : '0' }}</p>
                <p class="font-light">Категорий</p>
            </div>
        </div>
    </div>
    <!-- выставки -->
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Текущие выставки</p>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6" v-if="exhibitions && exhibitions.length > 0">
            <NuxtLink to="/" v-for="exhibition in exhibitions.slice(0,2)"
                class="group bg-gradient-to-b from-[#2C2C2C]/90 to-[#1C1C1C] rounded-2xl overflow-hidden transition-all duration-500 shadow-xl">
                <div class="relative overflow-hidden">
                    <img :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/images/${exhibition.image}`" :alt="exhibition.title"
                        class="w-full aspect-video object-cover brightness-75 transition-all duration-500 group-hover:brightness-100 group-hover:scale-105" />
                </div>
                <div class="p-6 flex flex-col gap-2">
                    <span class="text-xl font-semibold font-mono text-white">{{ exhibition.name }}</span>
                    <span class="font-light line-clamp-2">
                        {{ exhibition.desc }}
                    </span>
                    <span
                        class="flex bg-gradient-to-r from-cyan-500 to-cyan-600 text-white px-4 py-1.5 rounded-full font-medium transition-all duration-500 group-hover:-translate-y-1 group-hover:shadow-md w-fit">
                        Подробнее
                    </span>
                </div>
            </NuxtLink>
        </div>
        <div class="flex flex-col items-center justify-center text-center grow gap-6" v-else>
          <p class="mainHeading">Текущих выставок пока нет</p>
          <p>Все выставки доступны в каталоге</p>
          <NuxtLink to="/exhibitions" class="mx-auto px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">Перейти</NuxtLink>
        </div>
    </div>
    <!-- категории -->
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Популярные категории</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink v-for="category in categories.slice(0,4)" :key="category.id" :to="`/categories/category-${category.id}`"
                class="bg-[#2C2C2C]/90 shadow-md rounded-lg p-4 flex items-center flex-col gap-4 hover:shadow-lg transition-all duration-500 hover:bg-[#2C2C2C]">
                <Icon class="text-3xl text-cyan-500" :name="category.icon" />
                <span class="text-base font-medium">{{ category.name }}</span>
            </NuxtLink>
        </div>
    </div>
    <div class="bgStars w-full flex max-lg:flex-col gap-6 items-center text-center py-8 px-4 bg-[#2C2C2C]/90 rounded-2xl h-fit shadow-xl">
        <img src="/images/index/main.jpg" alt="" class="rounded-xl w-full lg:w-1/2">
        <div class="flex flex-col gap-4 w-full lg:w-1/2 items-center text-center">
            <p class="mainHeading">{{ authenticated ? 'Исследуй технологии' : 'Создавай выставки и сохраняй лучшие экспонаты' }}</p>
            <p class="font-light">Создавай выставки и сохраняй лучшие экспонаты</p>
            <NuxtLink :to="authenticated ? '/profile' : '/auth'" class="bg-gradient-to-r from-cyan-500 to-cyan-600 w-52 text-white transition-all duration-500 hover:-translate-y-1 hover:shadow-lg shadow-md px-4 py-1.5 rounded-full">
              {{ authenticated ? 'Присоединиться' : 'Начать' }}</NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* проверка роли и */
const userStore = useUserStore()
const { id:userId, role, authenticated } = useUserStore()


/* подключение БД */
const supabase = useSupabaseClient()


/* загрузка категорий */
const categories = ref([])
const loadCategories = async () => {
  const { data, error } = await supabase
  .from('categories')
  .select('*')
  .order('id', { ascending: Math.random() > 0.5 })

  if(data) {
    categories.value = data
  }
}


/* загрузка устройств */
const devices = ref([])
const loadDevices = async () => {
  const { data, error } = await supabase
  .from('devices')
  .select('*')

  if(data) {
    devices.value = data
  }
}


/* загрузка пользователей */
const users = ref([])
const loadUsers = async () => {
  const { data, error } = await supabase
  .from('users')
  .select('*')

  if(data) {
    users.value = data
  }
}


/* загрузка выставок */
const exhibitions = ref([])
const loadExhibitions = async () => {
  const now = new Date().toISOString()

  const { data, error } = await supabase
  .from('exhibitions')
  .select('*')
  .lte('start_date', now)
    .gte('end_date', now)
  .eq('is_published', true)

  if(data) {
    exhibitions.value = data
  }
}

// только для статистики
const exhibitionsStats = ref([])
const loadExhibitionsStats = async () => {
  const { data, error } = await supabase
  .from('exhibitions')
  .select('*')
  .eq('is_published', true)

  if(data) {
    exhibitionsStats.value = data
  }
}


/* первоначальная загрузка */
onMounted(() => {
  loadCategories()
  loadDevices()
  loadUsers()
  loadExhibitions()
  loadExhibitionsStats()
})
</script>