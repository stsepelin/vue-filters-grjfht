<script setup lang="ts">
import { reactiveOmit } from '@vueuse/core';
import {
  Label,
  SelectContent,
  SelectGroup,
  SelectItem,
  SelectItemIndicator,
  SelectItemText,
  SelectLabel,
  SelectPortal,
  SelectRoot,
  SelectScrollDownButton,
  SelectScrollUpButton,
  SelectSeparator,
  SelectTrigger,
  SelectValue,
  SelectViewport,
  useForwardPropsEmits,
} from 'radix-vue';
import {
  CheckIcon,
  ChevronUpIcon,
  ChevronDownIcon,
  ChevronUpDownIcon,
} from '@heroicons/vue/20/solid';
import type { SelectRootEmits, SelectRootProps } from 'radix-vue';

type SelectProps = SelectRootProps & {
  value: string;
  label: string;
  options: Array;
};

const props = defineProps<SelectProps>();
const emits = defineEmits<SelectRootEmits>();

const selectRootProps = reactiveOmit(props, ['label', 'id', 'options']);
const forward = useForwardPropsEmits(selectRootProps, emits);

// This is useful for usability purpose.
// Whenever a user clicks a label we would
// like to focus on the appropriate input
const filterId: string | number = props.id || Math.random();
</script>

<template>
  <Label
    class="block text-sm font-medium leading-6 text-gray-900"
    :for="filterId"
  >
    {{ props.label }}
  </Label>

  <div class="relative mt-2">
    <SelectRoot v-bind="forward">
      <SelectTrigger
        class="
          relative
          w-full
          cursor-default
          rounded-md
          bg-white
          py-1.5
          pl-3
          pr-10
          text-left text-gray-900
          shadow-sm
          ring-1 ring-inset ring-gray-300
          focus:outline-none focus:ring-2 focus:ring-indigo-600
          sm:text-sm sm:leading-6
        "
        :id="filterId"
        aria-label="Customise options"
      >
        <SelectValue placeholder="Select a fruit..." class="block truncate" />
        <span
          class="
            pointer-events-none
            absolute
            inset-y-0
            right-0
            flex
            items-center
            pr-2
          "
        >
          <ChevronUpDownIcon class="h-5 w-5 text-gray-400" aria-hidden="true" />
        </span>
      </SelectTrigger>

      <SelectPortal>
        <SelectContent
          class="
            absolute
            z-10
            mt-1
            max-h-60
            w-[var(--radix-select-trigger-width)]
            overflow-auto
            rounded-md
            bg-white
            py-1
            text-base
            shadow-lg
            ring-1 ring-black ring-opacity-5
            focus:outline-none
            sm:text-sm
          "
          position="popper"
          :side-offset="5"
        >
          <SelectScrollUpButton
            class="
              flex
              items-center
              justify-center
              h-[25px]
              bg-white
              text-violet11
              cursor-default
            "
          >
            <ChevronUpIcon class="h-5 w-5" aria-hidden="true" />
          </SelectScrollUpButton>

          <SelectViewport class="p-[5px]">
            <SelectItem
              v-for="option in props.options"
              :key="option.value"
              :value="option.value"
              class="relative cursor-default select-none py-2 pl-3 pr-9"
            >
              <SelectItemText class="block truncate">
                {{ option.label }}
              </SelectItemText>

              <SelectItemIndicator
                class="absolute inset-y-0 right-0 flex items-center pr-4"
              >
                <CheckIcon class="h-5 w-5" aria-hidden="true" />
              </SelectItemIndicator>
            </SelectItem>
          </SelectViewport>

          <SelectScrollDownButton
            class="
              flex
              items-center
              justify-center
              h-[25px]
              bg-white
              text-violet11
              cursor-default
            "
          >
            <ChevronDownIcon class="h-5 w-5" aria-hidden="true" />
          </SelectScrollDownButton>
        </SelectContent>
      </SelectPortal>
    </SelectRoot>
  </div>
</template>
