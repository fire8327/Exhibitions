<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Добавление устройства</p>
        <FormKit type="form" :actions="false" messages-class="hidden" form-class="w-full flex flex-col gap-6 items-center justify-center text-[#1C1C1C]">
            <FormKit v-model="deviceForm.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Наименование устройства" name="Наименование устройства" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="deviceForm.desc" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Описание устройства" name="Описание устройства" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit @change="(e) => { modelFile = e.target.files[0] }" validation="required" messages-class="text-[#E9556D] font-mono" type="file" label="Модель" label-class="text-white" name="Модель" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
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
const { id:userId, role, logout } = useUserStore()
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
    category_id: null
})


/* первоначальная загрузка */
onMounted(() => {
    loadCategories()
})
</script>