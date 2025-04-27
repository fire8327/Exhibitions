<template>
    <div class="flex flex-col gap-6">
        <p>Выставка: <span class="text-xl font-semibold text-white font-mono">{{ entity?.exhibitions.name }}</span></p>
        <p>Организатор: <span class="text-xl font-semibold text-white font-mono">{{ entity?.exhibitions.organizer }}</span></p>
        <p>Начало: <span class="text-xl font-semibold text-white font-mono">{{ new Date(entity?.exhibitions.start_date).toLocaleDateString() }}</span></p>
        <p>Конец: <span class="text-xl font-semibold text-white font-mono">{{ new Date(entity?.exhibitions.end_date).toLocaleDateString() }}</span></p>
        <img :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/images/${entity?.exhibitions.image}`" alt="" class="w-full rounded-xl aspect-video object-cover border border-white/10">
        <p class="tracking-widest">{{ entity?.exhibitions.desc }}</p>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Устройства выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 p-4 rounded-lg bg-[#2C2C2C] text-white" v-for="device in entity?.devices">
                <p>{{ device }}</p>
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
const entity = ref([])
const loadExhibition = async() => {
    const { data, error } = await supabase
    .from('exhibition_devices')
    .select('*, exhibitions(*), devices(*)')
    .eq('exhibition_id', route.params.id)


    if(data) {
        entity.value = data
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadExhibition()
})
</script>