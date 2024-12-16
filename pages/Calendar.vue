<template>
  <div class="text-center">
    <div>
      <p class="text-xl">
        {{ selectedDate }}
      </p>
    </div>
  </div>

  <cal-date
      v-model="selectedDate">
  </cal-date>
</template>

<script setup>
import { ref } from 'vue';

const selectedDate = ref(null);

</script>


