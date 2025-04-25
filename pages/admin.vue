<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Проверка устройств</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="device in devices">
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ device.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ device.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Категория: </span>{{ device.categories.name }}</p>
                <p><span class="text-white font-semibold font-mono">Подтверждено: </span>{{ device.is_approved ? 'Да' : 'Нет' }}</p>
            </div>
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Проверка выставок</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 p-4 rounded-xl shadow-lg bg-[#2C2C2C]" v-for="exhibition in exhibitions">
                <p><span class="text-white font-semibold font-mono">Наименование: </span>{{ exhibition.name }}</p>
                <p class="line-clamp-2 text-white font-semibold font-mono">{{ exhibition.desc }}</p>
                <p><span class="text-white font-semibold font-mono">Дата начала: </span>{{ new Date(exhibition.start_date).toLocaleDateString() }}</p>
                <p><span class="text-white font-semibold font-mono">Дата конца: </span>{{ new Date(exhibition.end_date).toLocaleDateString() }}</p>
                <p><span class="text-white font-semibold font-mono">Подтверждена: </span>{{ exhibition.is_published ? 'Да' : 'Нет' }}</p>
            </div>
        </div>
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


/* первоначальная загрузка */
onMounted(() => {
    loadDevices()
    loadExhibitions()
})
</script>