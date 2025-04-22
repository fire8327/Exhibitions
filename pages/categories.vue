<template>
    <div class="grow flex items-center justify-center">
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 w-full md:w-2/3">
            <NuxtLink v-for="category in data" to="/"
                class="bg-[#2C2C2C]/90 shadow-md rounded-lg p-4 flex items-center flex-col gap-4 hover:shadow-lg transition-all duration-500 hover:bg-[#2C2C2C] h-fit">
                <Icon class="text-3xl text-cyan-500" :name="category.icon" />
                <span class="text-base font-medium">{{ category.name }}</span>
            </NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* подключение БД */
const supabase = useSupabaseClient()

const { data, error } = await supabase
.from('categories')
.select()
</script>