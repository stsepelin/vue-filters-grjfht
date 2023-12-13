<script setup lang="ts">
import { defineAsyncComponent } from 'vue';

const props = defineProps({
  filters: Array,
});

const emit = defineEmits<{
  'filters:update': [value: string];
}>();

const components = {
  'input-type': defineAsyncComponent(
    () => import('@/components/filters/types/InputFilter.vue')
  ),
  'select-type': defineAsyncComponent(
    () => import('@/components/filters/types/SelectFilter.vue')
  ),
};

const filtersNormalized = {};

const handleFilterInputTypeUpdate = (
  { value }: { value: string },
  filter
): void => {
  filtersNormalized[filter.name] = value;

  emit('filters:update', { filtersNormalized: filtersNormalized });
};
</script>

<template>
  <section>
    <component
      v-for="filter in props.filters"
      :is="components[filter.type]"
      :params="filter.params"
      @filters:input:update="handleFilterInputTypeUpdate($event, filter)"
    />
  </section>
</template>
