<template>
    <div class="flex flex-col gap-6 grow">
        <div class="flex md:items-center md:justify-between gap-4 max-md:flex-col w-full">
            <p class="mainHeading">{{ category?.name }}</p>
            <NuxtLink @click="goBack"
                class="cursor-pointer max-md:order-first px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">
                Назад</NuxtLink>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" v-if="devices && devices.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-lg bg-[#2C2C2C] text-white" v-for="device in devices">
                <button @click="toggleFavorite(device.id)" class="cursor-pointer self-end" v-if="authenticated && role !=='admin' && role !=='creator'"> 
                    <Icon class="text-3xl transition-all duration-500" :name="isFavorite(device.id) ? 'material-symbols:heart-check-outline' : 'material-symbols:heart-plus-outline'" :class="isFavorite(device.id) ? 'text-red-500' : 'text-gray-400 hover:text-red-500'"/>
                </button>
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
        <div class="grow flex items-center justify-center text-center" v-else>
            <p class="mainHeading">Устройств в данной категории пока нет</p>
        </div>
    </div>
</template>

<script setup>
/* проверка роли и создание сообщений */
const { id:userId, role, authenticated } = useUserStore()
const { showMessage } = useMessagesStore()


/* возвращение назад */
const goBack = () => {
  if (window.history.state?.back) {
    useRouter().back();
  } else {
    useRouter().push('/'); // Или fallback-маршрут
  }
}


/* подключение БД и роут */
const supabase = useSupabaseClient()
const route = useRoute()


/* получение категории */
const category = ref(null)
const loadCategory = async() => {
    const { data, error } = await supabase
    .from('categories')
    .select()
    .eq('id', route.params.id)
    .single()

    if(data) {
        category.value = data
    }
}


/* получение устройств */
const devices = ref([])
const loadDevices = async() => {
    const { data, error } = await supabase
    .from('devices')
    .select()
    .eq('category_id', category.value.id)
    .eq('is_approved', 'true')

    if(data) {
        devices.value = data
    }
}


/* избранное */
const favorites = ref([])

// загрузка избранного пользователя
const loadFavorites = async () => {
    const { data, error } = await supabase
      .from('favorite_devices')
      .select('device_id')
      .eq('user_id', userId)
    
    if (data) {
      favorites.value = data.map(item => item.device_id)
    }
}

/* проверка, находится ли устройство в избранном */
const isFavorite = (deviceId) => {
  return favorites.value.includes(deviceId)
}

// переключение избранного
const toggleFavorite = async(deviceId) => {
    if (isFavorite(deviceId)) {
      // удаление из избранного
      const { error } = await supabase
        .from('favorite_devices')
        .delete()
        .eq('user_id', userId)
        .eq('device_id', deviceId)
      
      if (!error) {
        favorites.value = favorites.value.filter(id => id !== deviceId)
        showMessage('Удалено из избранного', true)
      } else {
        showMessage('Произошла ошибка', false)
      }
    } else {
      // добавление в избранное
      const { error } = await supabase
        .from('favorite_devices')
        .insert([
          { user_id: userId, device_id: deviceId }
        ])
      
      if (!error) {
        favorites.value = [...favorites.value, deviceId]
        showMessage('Добавлено в избранное', true)
      } else {
        showMessage('Произошла ошибка', false)
      }
    }
}


/* первоначальная загрузка */
onMounted(async () => {
    // импорт библиотеки для загрузки
    if (process.client) {
        await import('@google/model-viewer')
    }
    await loadCategory()
    await loadDevices()
    await loadFavorites()
})
</script>