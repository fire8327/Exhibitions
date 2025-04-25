<template>
      <div class="flex flex-col gap-6">
        <p class="mainHeading">Все выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" v-if="exhibitions && exhibitions.length > 0">
            <div v-for="exhibition in exhibitions"
                class="group bg-gradient-to-b from-[#2C2C2C]/90 to-[#1C1C1C] rounded-2xl overflow-hidden transition-all duration-500 shadow-xl">
                <div class="relative overflow-hidden">
                    <img :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/images/${exhibition.image}`" :alt="exhibition.name"
                        class="w-full aspect-video object-cover brightness-75 transition-all duration-500 group-hover:brightness-100 group-hover:scale-105"/>
                    <button @click="toggleFavorite(exhibition.id)" class="cursor-pointer absolute z-[1] top-4 right-4" v-if="authenticated && role !=='admin' && role !=='creator'"> 
                        <Icon class="text-3xl transition-all duration-500" :name="isFavorite(exhibition.id) ? 'material-symbols:heart-check-outline' : 'material-symbols:heart-plus-outline'" :class="isFavorite(exhibition.id) ? 'text-red-500' : 'text-gray-400 hover:text-red-500'"/>
                    </button>
                </div>
                <div class="p-6 flex flex-col gap-2">
                    <span class="text-xl font-semibold font-mono text-white">{{ exhibition.name }}</span>
                    <span class="font-light line-clamp-2">
                        {{ exhibition.desc }}
                    </span>
                    <NuxtLink to="/"
                        class="flex bg-gradient-to-r from-cyan-500 to-cyan-600 text-white px-4 py-1.5 rounded-full font-medium transition-all duration-500 group-hover:-translate-y-1 group-hover:shadow-md w-fit">
                        Подробнее
                    </NuxtLink>
                </div>
            </div>
        </div>
        <div class="flex flex-col items-center justify-center text-center grow gap-6" v-else>
          <p class="mainHeading">Выставок пока нет</p>
          <NuxtLink to="/" class="mx-auto px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">На главную</NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* проверка роли и создание сообщений */
const { id:userId, role, authenticated } = useUserStore()
const { showMessage } = useMessagesStore()


/* подключение БД */
const supabase = useSupabaseClient()

const exhibitions = ref([])
const loadExhibitions = async() => {
    const { data, error } = await supabase
    .from('exhibitions')
    .select()
    .eq('is_published', true)

    if(data) {
        exhibitions.value = data
    }
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
        showMessage('Удалено из избранного', true)
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
        showMessage('Добавлено в избранное', true)
      } else {
        showMessage('Произошла ошибка', false)
      }
    }
}


/* первоначальная загрузка */
onMounted(async () => {
    await loadExhibitions()
    await loadFavorites()
})
</script>