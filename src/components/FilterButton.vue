<script setup lang="ts">
import type { TaskFilter } from "../types";
const props = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();
const emits = defineEmits<{
  setFilter: [newFilter: TaskFilter];
}>();
</script>
<template>
  <button
    class="outline secondary"
    :class="{
      contrast: props.currentFilter === props.filter,
    }"
    @click="emits('setFilter', props.filter)"
  >
    {{ props.filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
