<script setup lang="ts">
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task, TaskFilter } from "./types";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";

const message = ref("Task Manager Application");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");

const totalCompleted = computed(() => {
  return tasks.value.filter((task) => task.completed).length;
});

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
    case "completed":
      return tasks.value.filter((task) => task.completed);
    case "pending":
      return tasks.value.filter((task) => !task.completed);
  }
  return tasks.value;
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

function setFilter(value: TaskFilter) {
  filter.value = value;
}
</script>

<template>
  <main>
    <h1>{{ message }}</h1>
    <TaskForm @add-task="addTask" />
    <h3 v-if="!tasks.length">Add a task to get started!</h3>
    <h3 v-else>{{ totalCompleted }} / {{ tasks.length }} tasks completed.</h3>
    <div v-if="tasks.length" class="button-container">
      <FilterButton
        :currentFilter="filter"
        filter="all"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="completed"
        @set-filter="setFilter"
      />
      <FilterButton
        :currentFilter="filter"
        filter="pending"
        @set-filter="setFilter"
      />
    </div>
    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style scoped>
main {
  max-width: 800px;
  margin: 1rem auto;
}
.button-container {
  display: flex;
  justify-content: end;
  gap: 0.5rem;
  margin-top: 1rem;
}
</style>
