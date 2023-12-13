<script setup lang="ts">
import { defineProps, ref, watch } from 'vue';

const props = defineProps({
  params: {
    label: String,
  },
});

const emit = defineEmits<{
  'filters:input:update': [value: string];
}>();

// This is useful for usability purpose.
// Whenever a user clicks a label we would
// like to focus on the appropriate input
const filterId: number = Math.random();

const value = ref('');

let timeoutHandler: number = -1;

watch(value, async (n) => {
  clearTimeout(timeoutHandler);

  // We do not want the input to be fired every time someone hits
  // a key so we will end up using a sort of debounce function here
  // this function is also known as 'poor man debounce function'
  // We could also use lodash debounce function
  timeoutHandler = setTimeout(() => {
    emit('filters:input:update', { value: n });
  }, 400);
});
</script>

<template>
  <div class="filters__input-type">
    <label class="filters__input-type__label" :for="filterId">
      {{ params.label }}
    </label>
    <input
      class="filters__input-type__input"
      v-model="value"
      type="text"
      :id="filterId"
    />
  </div>
</template>
