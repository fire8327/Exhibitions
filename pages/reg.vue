<template>
    <div class="flex flex-col gap-6 grow items-center justify-center">
        <FormKit @submit="regUser" type="form" :actions="false" messages-class="hidden" form-class="w-full flex flex-col gap-6 items-center justify-center text-[#1C1C1C] py-8 px-4 bg-[#2C2C2C] rounded-xl">
            <p class="mainHeading">Регистрация</p>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="user.surname" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Фамилия" name="Фамилия" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
                <FormKit v-model="user.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Имя" name="Имя" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            </div>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="user.login" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Логин" name="Логин" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
                <FormKit v-model="user.password" validation="required|length:6" messages-class="text-[#E9556D] font-mono" type="password" placeholder="······" name="Пароль" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            </div>
            <div class="flex max-md:flex-col items-center w-full md:w-2/3 lg:w-1/2 gap-4">
                <div @click="user.role = 'user'" class="cursor-pointer flex items-center gap-2 p-4 rounded-lg transition-all duration-500 w-full md:w-1/2 border-2 shadow-md" :class="[user.role === 'user' ? 'border-cyan-500 bg-[#000000]/15' : 'bg-[#3C3C3C] border-white/10 hover:opacity-50']">
                    <Icon class="text-3xl" name="ic:baseline-man" :class="[user.role === 'user' ? 'text-cyan-500' : 'text-white']"/>
                    <div class="flex flex-col gap-1">
                        <p class="font-semibold text-white">Пользователь</p>
                        <p class="text-sm text-[#A0A0A0]">Добавляйте гаджеты в избранное</p>
                    </div>
                </div>
                <div @click="user.role = 'creator'" class="cursor-pointer flex items-center gap-2 p-4 rounded-lg transition-all duration-500 w-full md:w-1/2 border-2 shadow-md" :class="[user.role === 'creator' ? 'border-cyan-500 bg-[#000000]/15' : 'bg-[#3C3C3C] border-white/10 hover:opacity-50']">
                    <Icon class="text-3xl" name="hugeicons:permanent-job" :class="[user.role === 'creator' ? 'text-cyan-500' : 'text-white']"/>
                    <div class="flex flex-col gap-1">
                        <p class="font-semibold text-white">Креатор</p>
                        <p class="text-sm text-[#A0A0A0]">Создавайте устройства и выставки</p>
                    </div>
                </div>
            </div>
            <FormKit v-model="user.email" validation="required|email" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Email" name="Email" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-cyan-500 shadow-md"/>
            <button :disabled="isRegDisabled" :class="{ 'opacity-50 cursor-not-allowed': isRegDisabled }" type="submit" class="px-4 py-2 border border-cyan-500 bg-cyan-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-cyan-500 hover:bg-transparent">Регистрация</button>
        </FormKit>
        <div class="w-full flex flex-col items-center justify-center gap-6 py-8 px-4 bg-[#2C2C2C] rounded-xl text-center">
            <p class="mainHeading max-w-md">Если у вас уже есть аккаунт, <span class="text-cyan-500">войдите</span> в систему</p>
            <NuxtLink to="/auth" class="mx-auto px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">Вход</NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Регистрация',
    lang: 'ru'
})


/* создание пользователя */
const user = ref({
    surname: "",
    name: "",
    avatar: "",
    login: "",
    password: "",
    email: "",
    role: "user"
})


/* создание сообщений */
const { showMessage } = useMessagesStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* регистрация пользователя */
const isRegDisabled = ref(false)
const regUser = async () => {
    isRegDisabled.value = true
    const { data: users, error: usersError } = await supabase
    .from('users')
    .select("*")
    .eq('login', user.value.login)

    if (users[0]) {
        user.value.login = ""
        isRegDisabled.value = false
        return showMessage("Такой  логин уже используется!", false)
    } 
    

    const { data, error } = await supabase
    .from('users')
    .insert(user.value)
    .select()

    if (data) {
        console.log(data)
        showMessage('Успешная регистрация!', true)
        isRegDisabled.value = false
        router.push('/auth')
    } else {
        console.log(user.value)            
        isRegDisabled.value = false
        showMessage('Произошла ошибка!', false)
    }
}
</script>