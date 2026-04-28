<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-task"]);
const newTask = ref("");

function handleAdd() {
  if (newTask.value.trim() === "") return;
  emit("add-task", newTask.value.trim());
  newTask.value = "";
}
</script>

<template>
  <div class="input-area">
    <input
      v-model="newTask"
      @keyup.enter="handleAdd"
      placeholder="New task..."
    />

    <button @click="handleAdd">Add task</button>
  </div>
</template>

<style scoped>
.input-area {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
  padding: 20px;
  background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
  border-radius: 15px;
  align-items: center;
}

input {
  flex: 1;
  padding: 14px 18px;
  font-size: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  font-family: "Poppins", sans-serif;
  transition: all 0.3s ease;
  background: white;
}

input:focus {
  outline: none;
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
  transform: scale(1.01);
}

input::placeholder {
  color: #cbd5e1;
  font-weight: 400;
}

button {
  padding: 14px 28px;
  cursor: pointer;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 12px;
  font-size: 0.95em;
  font-weight: 600;
  font-family: "Poppins", sans-serif;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 0 4px 15px rgba(102, 126, 234, 0.2);
  white-space: nowrap;
}

button:hover {
  transform: translateY(-3px);
  box-shadow: 0 8px 25px rgba(102, 126, 234, 0.35);
}

button:active {
  transform: translateY(-1px);
}
</style>
