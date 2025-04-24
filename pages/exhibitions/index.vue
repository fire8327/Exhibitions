<template>
      <div class="flex flex-col gap-6">
        <p class="mainHeading">Все выставки</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" v-if="data && data.length > 0">
            <NuxtLink to="/" v-for="exhibition in data"
                class="group bg-gradient-to-b from-[#2C2C2C]/90 to-[#1C1C1C] rounded-2xl overflow-hidden transition-all duration-500 shadow-xl">
                <div class="relative overflow-hidden">
                    <img :src="`https://qwcdjqzfdpcefrsxdktn.supabase.co/storage/v1/object/public/files/images/${exhibition.image}`" :alt="exhibition.name"
                        class="w-full aspect-video object-cover brightness-75 transition-all duration-500 group-hover:brightness-100 group-hover:scale-105" />
                </div>
                <div class="p-6 flex flex-col gap-2">
                    <span class="text-xl font-semibold font-mono text-white">{{ exhibition.name }}</span>
                    <span class="font-light line-clamp-2">
                        {{ exhibition.desc }}
                    </span>
                    <span
                        class="flex bg-gradient-to-r from-cyan-500 to-cyan-600 text-white px-4 py-1.5 rounded-full font-medium transition-all duration-500 group-hover:-translate-y-1 group-hover:shadow-md w-fit">
                        Подробнее
                    </span>
                </div>
            </NuxtLink>
        </div>
        <div class="flex flex-col items-center justify-center text-center grow gap-6" v-else>
          <p class="mainHeading">Выставок пока нет</p>
          <NuxtLink to="/" class="mx-auto px-4 py-2 border border-cyan-500 text-cyan-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-cyan-500">На главную</NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* подключение БД */
const supabase = useSupabaseClient()

const { data, error } = await supabase
.from('exhibitions')
.select()
.eq('is_published', true)
</script>