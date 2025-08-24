<script setup>
import { translations } from '../translations'

const emit = defineEmits(['update:locale'])

const updateLocale = (locale) => {
    emit('update:locale', locale)
}
</script>
<template>
    <select name="locale" id="locale" @change="(event) => updateLocale(event.target.value)">
        <option v-for="locale in Object.keys(translations)" :value="locale">{{ locale }}</option>
    </select>
</template>

<style scoped>
select {
    width: 50px;
    height: 20px;
}
</style>
