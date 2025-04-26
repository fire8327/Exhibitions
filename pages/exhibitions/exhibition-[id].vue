<template>
    <div class="flex flex-col gap-6">
        <p>Выставка: {{ exhibition }}</p>
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
const exhibition = ref(null)
const loadExhibition = async() => {
    const { data, error } = await supabase
    .from('exhibition_devices')
    .select('*, exhibitions(*), devices(*)')
    .eq('id', route.params.id)
    .single()

    if(data) {
        exhibition.value = data
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadExhibition()
})
</script>