<script setup>
import { translations } from '@/translations'
import { ref, computed } from 'vue'

const props = defineProps({
    date: String,
    locale: {
        type: String,
        default: 'ru',
    },
})

const emit = defineEmits(['day-change'])

const currentDate = ref(new Date(props.date))
const days = translations[props.locale].days
const months = translations[props.locale].months

if (currentDate.value.toString() === 'Invalid Date') {
    currentDate.value = new Date()
}
const currentMonth = ref(currentDate.value.getMonth())
const currentYear = ref(currentDate.value.getFullYear())
const currentDay = ref(currentDate.value.getDate())

const firstDayOfMonth = computed(() => {
    return new Date(currentYear.value, currentMonth.value, 1)
})
const lastDayOfMonth = computed(() => {
    return new Date(currentYear.value, currentMonth.value + 1, 0)
})

const startDay = computed(() => (firstDayOfMonth.value.getDay() + 6) % 7)

const nextMonth = () => {
    const month = currentMonth.value + 1
    currentYear.value = Math.floor(currentYear.value + month / 12)
    currentMonth.value = month % 12
}

const prevMonth = () => {
    const month = (12 + currentMonth.value - 1) % 12
    currentYear.value = currentYear.value - Math.floor(month / 11)
    currentMonth.value = month % 12
}
const onDayClick = (dayValue) => {
    currentDate.value = new Date(currentYear.value, currentMonth.value, dayValue)
    currentDay.value = dayValue
    emit('day-change', currentDate.value)
}
</script>

<template>
    <div class="calendar">
        <div class="calendar-header-year">
            <div @click="prevMonth" class="calendar-header-button no-select"><</div>
            <div>{{ months[currentMonth] }} {{ currentYear }}</div>
            <div @click="nextMonth" class="calendar-header-button no-select">></div>
        </div>
        <div class="calendar-header-days">
            <div class="calendar-header-day" v-for="day in days">{{ day }}</div>
        </div>
        <div class="calendar-body" :style="`--start-day: ${startDay}`">
            <div
                class="calendar-body-cell"
                v-for="day in lastDayOfMonth.getDate()"
                :class="
                    day === currentDay &&
                    currentMonth === currentDate.getMonth() &&
                    currentYear === currentDate.getFullYear()
                        ? 'selected'
                        : ''
                "
                @click="() => onDayClick(day)"
            >
                {{ day }}
            </div>
        </div>
    </div>
</template>

<style scoped>
.calendar {
    display: flex;
    flex-direction: column;
    border-radius: 6px;
    border: 2px solid #ccc;
    width: 250px;
    min-height: 250px;
    padding: 10px;
    gap: 10px;
}
.calendar-header-year {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
.calendar-header-days {
    display: flex;
    flex-direction: row;
}
.calendar-header-day {
    flex: 1;
    text-align: center;
}
.calendar-body {
    --start-day: 0;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
}
.calendar-body-cell {
    display: flex;
    flex: 0 0 calc(100% / 7);
    aspect-ratio: 1;
    text-align: center;
    box-sizing: border-box;
    border-radius: 4px;
    align-items: center;
    justify-content: center;
    cursor: pointer;
}
.calendar-body-cell:hover {
    border: 2px solid var(--primary-color);
}
.calendar-body-cell.selected {
    border: 2px solid var(--primary-color);
}
.calendar-body::before {
    content: '';
    flex: 0 0 calc(100% / 7 * var(--start-day, 0) - 2px);
}
.calendar-header-button {
    cursor: pointer;
}
</style>
