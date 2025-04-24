<template>
    <div class="flex flex-col gap-6">
        <div class="flex md:items-center md:justify-between gap-4 max-md:flex-col w-full">
            <p class="mainHeading">Добавление выставки</p>
            <NuxtLink to="/profile" class="max-md:order-first px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">Назад</NuxtLink>
        </div>
        <FormKit @submit="addDevice" type="form" :actions="false" messages-class="hidden" form-class="w-full flex flex-col gap-6 items-center justify-center text-[#1C1C1C]">
            <FormKit v-model="exhibitionForm.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Наименование выставки" name="Наименование выставки" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="exhibitionForm.desc" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Описание выставки" name="Описание выставки" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <FormKit v-model="exhibitionForm.organizer" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Организатор выставки" name="Организатор выставки" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <div class="flex items-center gap-2 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="exhibitionForm.start_date" validation="required" messages-class="text-[#E9556D] font-mono" type="date" label-class="text-white" label="Начало выставки" name="Начало выставки" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
                <FormKit v-model="exhibitionForm.end_date" validation="required" messages-class="text-[#E9556D] font-mono" type="date" label-class="text-white" label="Конец выставки" name="Конец выставки" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            </div>
            <FormKit @change="(e) => { imageFile = e.target.files[0] }" no-files-class="text-white" file-list-class="text-white" validation="required" messages-class="text-[#E9556D] font-mono" type="file" label="Изображение" label-class="text-white" name="Изображение" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
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


/* подключение бд и создание формы */
const supabase = useSupabaseClient()
const imageFile = ref(null)
const exhibitionForm = ref({
    name: "",
    desc: "",
    image: "",
    organizer: "",
    start_date: "",
    end_date: "",
    creator_id: userId    
})


/* добавление выставки */
const addDevice = async() => {
    isLoading.value = true
    if (imageFile.value) {
        const file = imageFile.value
        const extension = file.name.split('.').pop()
        const fileName = `${Date.now()}-${Math.random().toString(36).substring(2, 7)}.${extension}`

        const { error: uploadError } = await supabase.storage
        .from('files/images')
        .upload(`${fileName}`, file)

        if (uploadError) throw uploadError

        exhibitionForm.value.image = fileName

    }

    const { data, error } = await supabase
    .from('exhibitions')
    .insert(exhibitionForm.value)
    .select()

    if (data) {
        console.log(data)
        showMessage('Успешно добавлено!', true)
        isLoading.value = false
        router.push('/profile')
    } else {
        console.log(user.value)
        showMessage('Произошла ошибка!', false)
    }
}
</script>