<template>
    <div class="flex md:items-center md:justify-between gap-4 max-md:flex-col w-full">
        <p class="mainHeading">{{ device?.name }}</p>
        <NuxtLink @click="goBack"
            class="cursor-pointer max-md:order-first px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">
            Назад</NuxtLink>
    </div>
    <div class="w-full flex flex-col gap-4">
        <div class="relative w-full h-96 rounded-xl overflow-hidden shadow-lg">
            <ClientOnly>
                <model-viewer :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/models/${device?.file}`" camera-controls auto-rotate shadow-intensity="1"
                    class="w-full h-full block" style="background-color: #FFFFFF20;"
                    @load="() => console.log(`Модель загружена для устройства ${device.id}`)"
                    @error="(e) => console.error(`Ошибка загрузки модели для устройства ${device.id}:`, e)">
                    <div slot="poster" class="flex items-center justify-center h-full text-[#A0A0A0]">
                        Загрузка модели...
                    </div>
                </model-viewer>
            </ClientOnly>
            <button @click="toggleFavorite(device?.id)" class="cursor-pointer absolute z-[1] top-4 right-4" v-if="authenticated && role !=='admin' && role !=='creator'"> 
                <Icon class="text-3xl transition-all duration-500" :name="isFavorite(device?.id) ? 'material-symbols:heart-check-outline' : 'material-symbols:heart-plus-outline'" :class="isFavorite(device?.id) ? 'text-red-500' : 'text-gray-400 hover:text-red-500'"/>
            </button>
        </div>
        <p class="self-end"><span class="text-xl font-semibold font-mono text-white">Добавил устройство: </span>{{ device?.users?.surname }} {{ device?.users?.name }}</p>
    </div>
    <div class="flex flex-col gap-6">
        <div class="flex flex-col gap-4">
            <p class="text-xl font-semibold font-mono text-white">Описание</p>
            <p>{{ device?.desc }}</p>
        </div>
        <div class="flex max-md:flex-col md:items-center gap-4 md:gap-4">
            <p class="text-xl font-semibold font-mono text-white">Производитель</p>
            <p class="max-md:hidden text-white">-</p>
            <p>{{ device?.manufacturer }}</p>
        </div>
        <div class="flex max-md:flex-col md:items-center gap-4 md:gap-4">
            <p class="text-xl font-semibold font-mono text-white">Категория</p>
            <p class="max-md:hidden text-white">-</p>
            <p>{{ device?.categories?.name }}</p>
        </div>
        <div class="flex flex-col gap-4">
            <div class="flex items-center gap-2 w-full" v-for="characteristic in device?.characteristics">
                <p class="text-xl font-semibold font-mono text-white">{{ characteristic.key }}</p>
                <p class="grow border-b-[2px] border-dashed border-[#2C2C2C]"></p>
                <p>{{ characteristic.value }}</p>
            </div>
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
    useRouter().back()
  } else {
    useRouter().push('/') // или fallback-маршрут
  }
}


/* подключение БД и роут */
const supabase = useSupabaseClient()
const route = useRoute()


/* получение устройства */
const device = ref(null)
const loadDevice = async() => {
    const { data, error } = await supabase
    .from('devices')
    .select('*, users(surname, name), categories(name)')
    .eq('id', route.params.id)
    .eq('is_approved', 'true')
    .single()

    if(data) {
        device.value = data
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
    await loadDevice()
    await loadFavorites()
})
</script>