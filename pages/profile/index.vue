<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Личные данные</p>
        <FormKit @submit="saveProfile" type="form" :actions="false" messages-class="hidden" form-class="w-full flex flex-col gap-6 items-center justify-center text-[#1C1C1C]">
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="userForm.surname" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Фамилия" name="Фамилия" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
                <FormKit v-model="userForm.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Имя" name="Имя" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            </div>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="userForm.login" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Логин" name="Логин" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
                <FormKit v-model="userForm.password" validation="required|length:6" messages-class="text-[#E9556D] font-mono" type="password" placeholder="······" name="Пароль" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            </div>
            <FormKit v-model="userForm.email" validation="required|email" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Email" name="Email" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <button :disabled="isLoading" :class="{ 'opacity-50 cursor-not-allowed': isLoading }" type="submit" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Сохранить</button>
        </FormKit>
    </div>
    <div class="flex flex-col gap-6" v-if="role === 'creator'">
        <p class="mainHeading">Ваши устройства</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6" v-if="devices && devices.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="device in devices">
                <button @click="deleteDevice(device.id)" type="button" class="cursor-pointer self-end">
                    <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline"/>
                </button>
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ device.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ device.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Категория: </span>{{ device.categories.name }}</p>
            </div>
            <NuxtLink to="/profile/add-device" class="flex items-center justify-center gap-4 w-full py-6 rounded-xl shadow-lg transition-all duration-500 hover:opacity-60 bg-[#2C2C2C]">
                <Icon class="text-3xl" name="material-symbols:add-diamond-rounded"/>
                <span>Добавить</span>
            </NuxtLink>
        </div>
        <div class="flex flex-col gap-4 items-center justify-center text-center" v-else>
            <p class="text-xl font-mono font-semibold text-white">Пока нет устройств</p>
            <p>Начните создание коллекции, добавив первое устройство</p>
            <NuxtLink to="/profile/add-device" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Добавить</NuxtLink>
        </div>
    </div>
    <div class="flex flex-col gap-6" v-if="role === 'creator'">
        <p class="mainHeading">Ваши выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6" v-if="exhibitions && exhibitions.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="exhibition in exhibitions">
                <button @click="deleteExhibition(exhibition.id)" type="button" class="cursor-pointer self-end">
                    <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline"/>
                </button>
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ exhibition.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ exhibition.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Дата начала: </span>{{ new Date(exhibition.start_date).toLocaleDateString() }}</p>
                <p><span class="text-white font-semibold font-mono">Дата конца: </span>{{ new Date(exhibition.end_date).toLocaleDateString() }}</p>
            </div>
            <NuxtLink to="/profile/add-exhibition" class="flex items-center justify-center gap-4 w-full py-6 rounded-xl shadow-lg transition-all duration-500 hover:opacity-60 bg-[#2C2C2C]">
                <Icon class="text-3xl" name="material-symbols:add-diamond-rounded"/>
                <span>Добавить</span>
            </NuxtLink>
        </div>
        <div class="flex flex-col gap-4 items-center justify-center text-center" v-else>
            <p class="text-xl font-mono font-semibold text-white">Выставки не созданы</p>
            <p>Соберите свою первую выставку из добавленных устройств</p>
            <NuxtLink to="/profile/add-exhibition" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Создать</NuxtLink>
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Выход</p>
        <button @click="logout" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Выйти</button>
    </div>
</template>

<script setup>
/* проверка роли и создание сообщений */
const userStore = useUserStore()
const { id:userId, role, logout } = useUserStore()
const { showMessage } = useMessagesStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* загрузка */
const isLoading = ref(false)


/* форма пользователя */
const userForm = ref({
    name: '',
    surname: '',
    email: '',
    login: '',
    password: ''
})


/* получение данных профиля */
const loadProfileData = async() => {
    const { data, error } = await supabase
    .from('users')
    .select()
    .eq('id', userId)
    .single()

    if (error) throw error

    if (data) userForm.value = { ...data }
} 


/* сохранение\обновление профиля */
const saveProfile = async() => {
    const { error } = await supabase
    .from('users')
    .update(userForm.value)
    .eq('id', userId)

    if(error) {
        showMessage('Ошибка при сохранении', false)
    } else {
        showMessage('Профиль обновлён!', true)
    }
}


/* устройства */
const devices = ref([])

// добавление
const loadDevices = async() => {
    const { data, error } = await supabase
    .from('devices')
    .select('*, categories(*)')
    .eq('user_id', userId)

    if (error) throw error

    if (data) devices.value = data
}

// удаление
const deleteDevice = async(deviceId) => {
    try {
        // Получаем данные услуги
        const { data: deviceData, error: fetchError } = await supabase
            .from('devices')
            .select('file')
            .eq('id', deviceId)
            .single()

        if (fetchError) throw fetchError

        
        const { error: storageError } = await supabase.storage
            .from('files')
            .remove([`models/${deviceData.file}`])

        if (storageError) throw storageError
    

        // Удаляем запись из БД
        const { error: deleteError } = await supabase
            .from('devices')
            .delete()
            .eq('id', deviceId)

        if (deleteError) throw deleteError

        // Обновляем список
        await loadDevices()
        showMessage('Устройство удалено!', true)

    } catch (error) {
        console.error('Ошибка:', error)
        showMessage(`Ошибка удаления: ${error.message}`, false)
    }
}


/* выставки */
const exhibitions = ref([])

// добавление
const loadExhibitions = async() => {
    const { data, error } = await supabase
    .from('exhibitions')
    .select()
    .eq('creator_id', userId)

    if (error) throw error

    if (data) exhibitions.value = data
}

//удаление
const deleteExhibition = async(exhibitionId) => {
    try {
        // Получаем данные услуги
        const { data: exhibitionData, error: fetchError } = await supabase
            .from('exhibitions')
            .select('image')
            .eq('id', exhibitionId)
            .single()

        if (fetchError) throw fetchError

        
        const { error: storageError } = await supabase.storage
            .from('files')
            .remove([`images/${exhibitionData.file}`])

        if (storageError) throw storageError
    

        // Удаляем запись из БД
        const { error: deleteError } = await supabase
            .from('exhibitions')
            .delete()
            .eq('id', exhibitionId)

        if (deleteError) throw deleteError

        // Обновляем список
        await loadExhibitions()
        showMessage('Выставка удалена!', true)

    } catch (error) {
        console.error('Ошибка:', error)
        showMessage(`Ошибка удаления: ${error.message}`, false)
    }
}



/* первоначальная загрузка */
onMounted(() => {
    loadProfileData()
    loadDevices()
    loadExhibitions()
})
</script>