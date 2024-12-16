<template>
  <div class="bg-gray-100 flex justify-center items-center min-h-screen">
    <div class="w-full max-w-md bg-white shadow-lg rounded-lg overflow-hidden">
      <div class="bg-blue-600 text-white text-center py-4">
        <div class="flex justify-center gap-[10px]">
          <img class="cursor-pointer w-[30px] h-[30px]" src="/images/arrow_back.png" alt="Back" @click="changeMonth(-1)"/>
          <h1 class="text-xl font-semibold cursor-pointer">
            {{ monthName }} {{ date.year }}
          </h1>
          <img class="cursor-pointer w-[30px] h-[30px]" src="/images/arrow_forward.png" alt="Forward" @click="changeMonth(1)"/>
        </div>
      </div>
      <div class="grid grid-cols-7 text-center font-medium text-gray-700">
        <div v-for="(week, index) in date.weekDays" :key="index" class="py-2 cursor-pointer">
          <p>{{ week }}</p>
        </div>
      </div>
      <div class="grid grid-cols-7 text-center text-gray-900">
        <div v-for="(day, index) in date.days" :key="index" class="cursor-pointer py-2">
          <p @click="setDay(day)">
            {{ day }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watchEffect } from 'vue';
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue']);

const date = ref({
  days: [],
  weekDays: ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
  monthNames: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
  year: new Date().getFullYear()
});

const monthIndex = ref(new Date().getMonth());
const monthName = computed(() => date.value.monthNames[monthIndex.value]);

function changeMonth(direction) {
  monthIndex.value = (monthIndex.value + direction + 12) % 12;
  if (direction > 0 && monthIndex.value === 0) date.value.year++;
  if (direction < 0 && monthIndex.value === 11) date.value.year--;
}

function setDay(day) {
  emit('update:modelValue', new Date(date.value.year, monthIndex.value, day));
}

watchEffect(() => {
  const firstDay = new Date(date.value.year, monthIndex.value, 1).getDay();
  const daysInMonth = new Date(date.value.year, monthIndex.value + 1, 0).getDate();

  date.value.days = Array.from({ length: firstDay }, () => null).concat(
      Array.from({ length: daysInMonth }, (_, i) => i + 1)
  );
});
</script>
