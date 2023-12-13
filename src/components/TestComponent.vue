<script setup lang="ts">
import Filters from '@/components/filters/Filters.vue';

const filters = [
  {
    type: 'input-type',
    name: 'userId',
    params: {
      label: 'User ID',
    },
  },
  {
    type: 'input-type',
    name: 'userName',
    params: {
      label: 'User Name',
    },
  },
  {
    type: 'select-type',
    name: 'userCity',
    params: {
      label: 'User City',
      options: [
        { label: 'Tallinn', value: '1' },
        { label: 'Loksa', value: '2' },
        { label: 'Helsinki', value: '3' },
      ],
    },
  },
];

const handleFiltersUpdate = ({ filtersNormalized }) => {
  console.log(filtersNormalized);
};
</script>

<template>
  <div class="max-w-md">
    <div class="grid gap-4">
      <Filters :filters="filters" @filters:update="handleFiltersUpdate" />
    </div>
  </div>
</template>
