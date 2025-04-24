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
        </div>
        <p class="self-end"><span class="text-xl font-semibold font-mono text-white">Добавил устройство: </span>{{ device?.users?.surname }} {{ device?.users?.name }}</p>
    </div>
    <div class="flex flex-col gap-6">
        <p class="text-xl font-semibold font-mono text-white">Описание</p>
        <p>{{ device?.desc }}</p>
    </div>
    <div class="flex max-md:flex-col md:items-center gap-6 md:gap-4">
        <p class="text-xl font-semibold font-mono text-white">Производитель</p>
        <p class="max-md:hidden text-white">-</p>
        <p>{{ device?.manufacturer }}</p>
    </div>
    <div class="flex max-md:flex-col md:items-center gap-6 md:gap-4">
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
</template>

<script setup>
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


/* первоначальная загрузка */
onMounted(async () => {
    // импорт библиотеки для загрузки
    if (process.client) {
        await import('@google/model-viewer')
    }
    await loadDevice()
})
</script>