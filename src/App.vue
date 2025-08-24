<script setup>
import Calendar from '@/components/Calendar.vue'
import LanguageChanger from '@/components/LanguageChanger.vue'
import { ref } from 'vue'

const currentLocale = ref('en')

const onLocaleChange = (locale) => {
    currentLocale.value = locale
    console.log(currentLocale.value)
}
</script>

<template>
    <main class="main">
        <LanguageChanger @update:locale="(locale) => onLocaleChange(locale)" />
        <Calendar
            :locale="currentLocale"
            @day-change="
                (date) => {
                    console.log(date)
                }
            "
            :key="'calendar-' + currentLocale"
        />
    </main>
</template>

<style scoped>
.main {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}
</style>
