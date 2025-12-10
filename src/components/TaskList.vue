<script setup lang="ts">
import type { Task } from "../types";

const props = defineProps<{ tasks: Task[] }>();

const emits = defineEmits<{
  toggleDone: [id: string];
  removeTask: [id: string];

}>();
</script>
<template>
  <TransitionGroup name="list" tag="div" class="task-list">
    <article v-for="task in props.tasks" :key="task.id" class="task">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          :checked="task.completed"
          type="checkbox"
        />
        <span :class="{ done: task.completed }">{{ task.title }}</span>
      </label>
      <button @click="emits('removeTask', task.id)" class="outline">
        Remove!
      </button>
    </article>
  </TransitionGroup>
</template>

<style scoped>
.task-list {
  margin-top: 2rem;
  
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #0d5086;
}

.done {
  text-decoration: line-through;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}

.list-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}

.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-move {
  transition: transform 0.9s ease;
}
</style>
