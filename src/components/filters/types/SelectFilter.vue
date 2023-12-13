<script setup lang="ts">
import { defineProps, ref, watch } from 'vue';
import SelectInput from '@/components/inputs/SelectInput.vue';

const props = defineProps({
  params: {
    label: String,
    options: Array,
  },
});

const emit = defineEmits<{
  'filters:input:update': [value: string];
}>();

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
  <SelectInput
    v-model:value="value"
    :label="props.params.label"
    :options="props.params.options"
  />
</template>
