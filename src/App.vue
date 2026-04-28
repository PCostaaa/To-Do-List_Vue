<script setup>
import { computed, ref, watch } from "vue";
import TaskInput from "./components/TaskInput.vue";
import TaskItem from "./components/TaskItem.vue";

const tasks = ref(JSON.parse(localStorage.getItem("tasks") || "[]"));
const filter = ref("all");

const filteredTasks = computed(() => {
  if (filter.value === "active") return tasks.value.filter((t) => !t.completed);
  if (filter.value === "completed")
    return tasks.value.filter((t) => t.completed);
  return tasks.value;
});

function addTask(text) {
  tasks.value.push({
    id: Date.now(),
    text,
    completed: false,
  });
}

function editTask(id, newText) {
  const task = tasks.value.find((t) => t.id === id);
  if (task) task.text = newText;
}

function removeTask(id) {
  tasks.value = tasks.value.filter((t) => t.id !== id);
}

function toggleTask(id) {
  const task = tasks.value.find((t) => t.id === id);
  if (task) task.completed = !task.completed;
}

watch(
  tasks,
  (newVal) => {
    localStorage.setItem("tasks", JSON.stringify(newVal));
  },
  { deep: true },
);
</script>

<template>
  <div class="container">
    <h1>To-Do-List</h1>

    <TaskInput @add-task="addTask" />

    <div class="filters">
      <button
        v-for="option in ['all', 'active', 'completed']"
        :key="option"
        :class="{ active: filter === option }"
        @click="filter = option"
      >
        {{ option }}
      </button>
    </div>

    <ul>
      <TaskItem
        v-for="task in filteredTasks"
        :key="task.id"
        :task="task"
        @toggle-task="toggleTask"
        @remove-task="removeTask"
        @edit-task="editTask"
      />
    </ul>

    <p v-if="tasks.length === 0">No tasks yet!</p>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 30px auto;
  background: white;
  border-radius: 20px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  animation: slideUp 0.6s ease-out;
}

@keyframes slideUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

h1 {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 30px 20px;
  font-size: 2.5em;
  text-align: center;
  font-weight: 700;
  letter-spacing: -0.5px;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.2);
}

ul {
  list-style: none;
  padding: 20px;
  max-height: 400px;
  overflow-y: auto;
}

ul::-webkit-scrollbar {
  width: 8px;
}

ul::-webkit-scrollbar-track {
  background: #f1f5f9;
}

ul::-webkit-scrollbar-thumb {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 4px;
}

p {
  text-align: center;
  color: #9ca3af;
  padding: 40px 20px;
  font-size: 1.1em;
  font-weight: 300;
}

.filters {
  display: flex;
  gap: 12px;
  margin: 20px 20px;
  padding: 15px;
  background: #f8fafc;
  border-radius: 12px;
  flex-wrap: wrap;
  justify-content: center;
}

.filters button {
  padding: 10px 24px;
  cursor: pointer;
  border: 2px solid #e2e8f0;
  background: white;
  border-radius: 25px;
  font-size: 0.95em;
  font-weight: 500;
  font-family: "Poppins", sans-serif;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  color: #64748b;
}

.filters button:hover {
  border-color: #667eea;
  color: #667eea;
  transform: translateY(-2px);
}

.filters button.active {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-color: transparent;
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.3);
}
</style>
