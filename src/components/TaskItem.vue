<script setup>
import { nextTick, ref } from "vue";

const props = defineProps({
  task: Object,
});

const emit = defineEmits(["toggle-task", "remove-task", "edit-task"]);

const isEditing = ref(false);
const editText = ref("");
const editInput = ref(null);

async function startEditing() {
  isEditing.value = true;
  editText.value = props.task.text;
  await nextTick();
  editInput.value.focus();
}

function saveEdit() {
  if (editText.value.trim() === "") return;
  emit("edit-task", props.task.id, editText.value.trim());
  isEditing.value = false;
}

function cancelEdit() {
  isEditing.value = false;
}
</script>

<template>
  <li>
    <input
      v-if="isEditing"
      ref="editInput"
      v-model="editText"
      class="edit-input"
      @keyup.enter="saveEdit"
      @keyup.escape="cancelEdit"
      @blur="saveEdit"
    />
    <span
      v-else
      @click="emit('toggle-task', task.id)"
      @dblclick="startEditing"
      :style="
        task.completed ? { textDecoration: 'line-through', color: '#aaa' } : {}
      "
    >
      {{ task.text }}
    </span>

    <button @click="emit('remove-task', task.id)">Delete</button>
  </li>
</template>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 16px;
  margin-bottom: 12px;
  background: white;
  border: 2px solid #e2e8f0;
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  animation: fadeIn 0.4s ease-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateX(-10px);
  }
  to {
    opacity: 1;
    transform: translateX(0);
  }
}

li:hover {
  border-color: #667eea;
  box-shadow: 0 4px 12px rgba(102, 126, 234, 0.15);
  transform: translateX(4px);
}

span {
  flex: 1;
  font-size: 1em;
  font-weight: 500;
  color: #1e293b;
  word-break: break-word;
  transition: all 0.3s ease;
}

span:hover {
  color: #667eea;
}

.edit-input {
  flex: 1;
  padding: 10px 12px;
  font-size: 1em;
  border: 2px solid #667eea;
  border-radius: 8px;
  font-family: "Poppins", sans-serif;
  transition: all 0.2s ease;
  background: #f8fafc;
}

.edit-input:focus {
  outline: none;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.2);
}

button {
  padding: 8px 16px;
  cursor: pointer;
  margin-left: 12px;
  background: #ef4444;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 0.85em;
  font-weight: 600;
  font-family: "Poppins", sans-serif;
  transition: all 0.3s ease;
  white-space: nowrap;
}

button:hover {
  background: #dc2626;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
}

button:active {
  transform: translateY(0);
}
</style>
