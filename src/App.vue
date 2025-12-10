<script setup lang="ts">
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task } from "./types";
import TaskList from "./components/TaskList.vue";

const message = ref("Task Manager Application");
const tasks = ref<Task[]>([]);

const totalCompleted = computed(() => {
  return tasks.value.filter((task) => task.completed).length;
});

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    completed: false,
  });
}

function toggleDone(id: string) {
  const task = tasks.value.find((task) => task.id === id);
  if (task) {
    task.completed = !task.completed;
  }
}

function removeTask(id: string) {
  const index = tasks.value.findIndex((task) => task.id === id);
  if (index !== -1) {
    tasks.value.splice(index, 1);
  }
  if (tasks.value.length === 0) {
    message.value = "All tasks completed! Add more tasks to stay productive.";
  }
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started!</h3>
    <h3 v-else>{{ totalCompleted }} / {{ tasks.length }} tasks completed.</h3>
    <TaskList :tasks @toggle-done="toggleDone" @remove-task="removeTask" />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
}
</style>
