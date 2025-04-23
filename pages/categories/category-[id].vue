<template>
    <div class="flex flex-col gap-6 grow">
        <div class="flex md:items-center md:justify-between gap-4 max-md:flex-col w-full">
            <p class="mainHeading">{{ category?.name }}</p>
            <NuxtLink to="/categories"
                class="max-md:order-first px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">
                Назад</NuxtLink>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" v-if="devices && devices.length > 0">
            <div class="flex flex-col gap-4 p-4 rounded-lg bg-[#2C2C2C]" v-for="device in devices">
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
            </div>
        </div>
        <div class="grow flex items-center justify-center text-center" v-else>
            <p class="mainHeading">Устройств в данной категории нет</p>
        </div>
    </div>
</template>

<script setup>
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


/* первоначальная загрузка */
onMounted(async () => {
    // импорт библиотеки для загрузки
    if (process.client) {
        await import('@google/model-viewer')
    }
    await loadCategory()
    await loadDevices()
})
</script>