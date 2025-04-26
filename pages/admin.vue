<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Проверка устройств</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6" v-if="devices && devices.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="device in devices">
                <div class="flex items-center gap-2 self-end">
                    <button @click="completeEntity('devices', device.id)" class="cursor-pointer" v-if="!device.is_approved">
                        <Icon class="text-3xl text-green-500" name="material-symbols:check-rounded"/>
                    </button>
                    <button @click="deleteEntity('devices', device.id)" class="cursor-pointer">
                        <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline"/>
                    </button>
                </div>
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ device.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ device.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Категория: </span>{{ device.categories.name }}</p>
                <p><span class="text-white font-semibold font-mono">Подтверждено: </span>{{ device.is_approved ? 'Да' : 'Нет' }}</p>
            </div>
        </div>
        <p v-else class="text-xl font-semibold font-mono text-center">Устройств нет</p>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Проверка выставок</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6" v-if="exhibitions && exhibitions.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="exhibition in exhibitions">
                <div class="flex items-center gap-2 self-end">
                    <button @click="completeEntity('exhibitions', exhibition.id)" class="cursor-pointer" v-if="!exhibition.is_published">
                        <Icon class="text-3xl text-green-500" name="material-symbols:check-rounded"/>
                    </button>
                    <button @click="deleteEntity('exhibitions', exhibition.id)" class="cursor-pointer">
                        <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline"/>
                    </button>
                </div>
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ exhibition.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ exhibition.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Дата начала: </span>{{ new Date(exhibition.start_date).toLocaleDateString() }}</p>
                <p><span class="text-white font-semibold font-mono">Дата конца: </span>{{ new Date(exhibition.end_date).toLocaleDateString() }}</p>
                <p><span class="text-white font-semibold font-mono">Подтверждена: </span>{{ exhibition.is_published ? 'Да' : 'Нет' }}</p>
            </div>
        </div>
        <p v-else class="text-xl font-semibold font-mono text-center">Выставок нет</p>
    </div>
</template>

<script setup>
/* создание сообщений и подключение бд */
const { showMessage } = useMessagesStore()
const supabase = useSupabaseClient()


/* устройства */
const devices = ref([])
const loadDevices = async() => {
    const { data, error } = await supabase
    .from('devices')
    .select('*, categories(*)')

    if(data) {
        devices.value = data
    }
}


/* выставки */
const exhibitions = ref([])
const loadExhibitions = async() => {
    const { data, error } = await supabase
    .from('exhibitions')
    .select()

    if(data) {
        exhibitions.value = data
    }
}


/* подвтерждение и удаление записей */
//подтверждение
const completeEntity = async(table, entityId) => {
    const updateData = table === 'devices' 
    ? { is_approved: true } 
    : { is_published: true }

    const { data, error } = await supabase
    .from(table)
    .update(updateData)
    .eq('id', entityId)
    .select()

    if(error) {
        showMessage('Ошибка при сохранении', false)
    } else {
        showMessage('Статус обновлён!', true)
        loadDevices()
        loadExhibitions()
    }
}

//удаление
const deleteEntity = async(table, entityId) => {
    const { error } = await supabase
    .from(table)
    .delete()
    .eq('id', entityId)

    if(error) {
        showMessage('Ошибка при сохранении', false)
    } else {
        showMessage('Успешное удаление!', true)
        loadDevices()
        loadExhibitions()
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadDevices()
    loadExhibitions()
})
</script>