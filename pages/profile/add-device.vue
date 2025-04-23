<template>
    <div class="flex flex-col gap-6">
        <div class="flex md:items-center md:justify-between gap-4 max-md:flex-col w-full">
            <p class="mainHeading">Добавление устройства</p>
            <NuxtLink to="/profile" class="max-md:order-first px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">Назад</NuxtLink>
        </div>
        <FormKit @submit="addDevice" type="form" :actions="false" messages-class="hidden" form-class="w-full flex flex-col gap-6 items-center justify-center text-[#1C1C1C]">
            <FormKit v-model="deviceForm.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Наименование устройства" name="Наименование устройства" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="deviceForm.desc" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Описание устройства" name="Описание устройства" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="deviceForm.manufacturer" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Производитель" name="Производитель" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit @change="(e) => { modelFile = e.target.files[0] }" no-files-class="text-white" file-list-class="text-white" validation="required" messages-class="text-[#E9556D] font-mono" type="file" label="Модель" label-class="text-white" name="Модель" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="deviceForm.category_id" validation="required" messages-class="text-[#E9556D] font-mono" type="select" :options="categoryOptions" placeholder="Категория" name="Категория" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <button :disabled="isLoading" :class="{ 'opacity-50 cursor-not-allowed': isLoading }" type="submit" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Добавить</button>
        </FormKit>
    </div>
</template>

<script setup>
/* создание роутера и статус загрузки */
const router = useRouter()
const isLoading = ref(false)


/* проверка роли и создание сообщений */
const { id:userId, role } = useUserStore()
const { showMessage } = useMessagesStore()


/* получение доступных категорий */
const categories = ref([])
const loadCategories = async() => {
    const { data, error } = await supabase
    .from('categories')
    .select('id, name')

    if (error) throw error

    if (data) categories.value = data
}

// обработка уже полученных категорий
const categoryOptions = computed(() => {
  return categories.value.map(category => ({
    value: category.id,  // это будет передано в v-model
    label: category.name // это будет отображаться в select
  }))
})


/* подключение бд и создание формы */
const supabase = useSupabaseClient()
const modelFile = ref(null)
const deviceForm = ref({
    name: "",
    desc: "",
    file: "",
    category_id: null,
    manufacturer: "",
    user_id: userId
})


/* добавление товара */
const addDevice = async() => {
    if (modelFile.value) {
        const file = modelFile.value
        const extension = file.name.split('.').pop()
        const fileName = `${Date.now()}-${Math.random().toString(36).substring(2, 7)}.${extension}`

            
        const { error: uploadError } = await supabase.storage
        .from('files/models')
        .upload(`${fileName}`, file)

        if (uploadError) throw uploadError

        deviceForm.value.file = fileName

    }

    const { data, error } = await supabase
    .from('devices')
    .insert(deviceForm.value)
    .select()

    if (data) {
        console.log(data)
        showMessage('Успешно добавлено!', true)
        router.push('/profile')
    } else {
        console.log(user.value)
        showMessage('Произошла ошибка!', false)
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadCategories()
})
</script>