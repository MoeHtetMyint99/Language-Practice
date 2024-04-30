<script setup lang="ts">
import { computed } from 'vue'

defineProps<{
  color: 'red' | 'blue' | 'green'
}>()

const colorVariants = computed(() => ({
  red: 'bg-red-700 hover:bg-red-800 focus:ring-red dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-800',
  blue: 'bg-blue-700 hover:bg-blue-800 focus:ring-blue dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800',
  green:
    'bg-green-700 hover:bg-green-800 focus:ring-green dark:bg-green-600 dark:hover:bg-green-700 dark:focus:ring-green-800'
}))
</script>

<template>
  <button
    :class="`text-white focus:ring-4-300 font-medium rounded-lg text-sm p-2 lg:px-4 lg:py-2.5 focus:outline-none ${colorVariants[color]}`"
  >
    <slot />
  </button>
</template>
