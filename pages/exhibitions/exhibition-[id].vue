<template>
    <div class="flex flex-col gap-6">
        <button @click="toggleFavorite(exhibition?.id)" class="cursor-pointer self-end" v-if="authenticated && role !=='admin' && role !=='creator'"> 
            <Icon class="text-3xl transition-all duration-500" :name="isFavorite(exhibition?.id) ? 'material-symbols:heart-check-outline' : 'material-symbols:heart-plus-outline'" :class="isFavorite(exhibition?.id) ? 'text-red-500' : 'text-gray-400 hover:text-red-500'"/>
        </button>
        <p>Выставка: <span class="text-xl font-semibold text-white font-mono">{{ exhibition?.name }}</span></p>
        <p>Организатор: <span class="text-xl font-semibold text-white font-mono">{{ exhibition?.organizer }}</span></p>
        <p>Начало: <span class="text-xl font-semibold text-white font-mono">{{ new Date(exhibition?.start_date).toLocaleDateString() }}</span></p>
        <p>Конец: <span class="text-xl font-semibold text-white font-mono">{{ new Date(exhibition?.end_date).toLocaleDateString() }}</span></p>
        <img :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/images/${exhibition?.image}`" alt="" class="w-full rounded-xl aspect-video object-cover border border-white/10">
        <p class="tracking-widest">{{ exhibition?.desc }}</p>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Устройства выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 p-4 rounded-lg bg-[#2C2C2C] text-white" v-for="device in devices">
                <p>{{ device.name }}</p>
                <p>{{ device.desc }}</p>
                <div class="relative w-full h-96 rounded-md overflow-hidden bg-gradient-to-b from-[#2C2C2C]/90 to-[#1A1A1A] shadow-lg">
                    <ClientOnly>
                        <model-viewer :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/models/${device.file}`" camera-controls auto-rotate shadow-intensity="1"
                            class="w-full h-full block" style="background-color: #1A1A1A;"
                            @load="() => console.log(`Модель загружена для устройства ${device.id}`)"
                            @error="(e) => console.error(`Ошибка загрузки модели для устройства ${device.id}:`, e)">
                            <div slot="poster" class="flex items-center justify-center h-full text-[#A0A0A0]">
                                Загрузка модели...
                            </div>
                        </model-viewer>
                    </ClientOnly>
                </div>
                <NuxtLink :to="`/devices/device-${device.id}`"
                class="px-4 py-2 border border-cyan-500 hover:text-cyan-500 rounded-full w-full text-center transition-all duration-500 text-white bg-cyan-500 hover:bg-transparent">
                Подробнее</NuxtLink>
            </div>
        </div>
    </div>
</template>

<script setup>
/* проверка роли и создание сообщений */
const { id:userId, role, authenticated } = useUserStore()
const { showMessage } = useMessagesStore()


/* роут и подключение бд */
const route = useRoute()
const supabase = useSupabaseClient()


/* получение выставки */
const exhibition = ref(null)
const devices = ref([])
const loadData = async() => {
    const { data: exhibitionData, error: exhibitionError } = await supabase
    .from('exhibitions')
    .select('*, exhibition_devices(devices(*))')
    .eq('id', route.params.id)
    .single()

    if(exhibitionData) {
        exhibition.value = exhibitionData
    }

    devices.value = exhibitionData.exhibition_devices
    .map((ed) => ed.devices)
}


/* избранное */
const favorites = ref([])

// загрузка избранного пользователя
const loadFavorites = async () => {
    const { data, error } = await supabase
      .from('favorite_exhibitions')
      .select('exhibition_id')
      .eq('user_id', userId)
    
    if (data) {
      favorites.value = data.map(item => item.exhibition_id)
    }
}

/* проверка, находится ли устройство в избранном */
const isFavorite = (exhibitionId) => {
  return favorites.value.includes(exhibitionId)
}

// переключение избранного
const toggleFavorite = async(exhibitionId) => {
    if (isFavorite(exhibitionId)) {
      // удаление из избранного
      const { error } = await supabase
        .from('favorite_exhibitions')
        .delete()
        .eq('user_id', userId)
        .eq('exhibition_id', exhibitionId)
      
      if (!error) {
        favorites.value = favorites.value.filter(id => id !== exhibitionId)
        showMessage('Удалена из избранного', true)
      } else {
        showMessage('Произошла ошибка', false)
      }
    } else {
      // добавление в избранное
      const { error } = await supabase
        .from('favorite_exhibitions')
        .insert([
          { user_id: userId, exhibition_id: exhibitionId }
        ])
      
      if (!error) {
        favorites.value = [...favorites.value, exhibitionId]
        showMessage('Добавлена в избранное', true)
      } else {
        showMessage('Произошла ошибка', false)
      }
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadData()
    loadFavorites()
})
</script>