<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const api = axios.create({ baseURL: 'http://localhost:8000/api' })

const tasks = ref([])
const newTitle = ref('')
const newDescription = ref('')
const editingTask = ref(null)

async function fetchTasks() {
  const res = await api.get('/tasks')
  tasks.value = res.data
}

async function addTask() {
  if (!newTitle.value.trim()) return
  await api.post('/tasks', { title: newTitle.value, description: newDescription.value })
  newTitle.value = ''
  newDescription.value = ''
  fetchTasks()
}

async function toggleComplete(task) {
  await api.put(`/tasks/${task.id}`, { completed: !task.completed })
  fetchTasks()
}

async function deleteTask(id) {
  await api.delete(`/tasks/${id}`)
  fetchTasks()
}

function startEdit(task) {
  editingTask.value = { ...task }
}

async function saveEdit() {
  await api.put(`/tasks/${editingTask.value.id}`, editingTask.value)
  editingTask.value = null
  fetchTasks()
}

onMounted(fetchTasks)
</script>

<template>
  <div class="container">
    <h1>TODO List</h1>

    <div class="form">
      <input v-model="newTitle" placeholder="Názov úlohy" />
      <input v-model="newDescription" placeholder="Popis (nepovinný)" />
      <button @click="addTask">Pridať</button>
    </div>

    <ul>
      <li v-for="task in tasks" :key="task.id">
        <div v-if="editingTask && editingTask.id === task.id">
          <input v-model="editingTask.title" />
          <input v-model="editingTask.description" />
          <button @click="saveEdit">Uložiť</button>
          <button @click="editingTask = null">Zrušiť</button>
        </div>
        <div v-else>
          <span :class="{ done: task.completed }">{{ task.title }}</span>
          <small>{{ task.description }}</small>
          <button @click="toggleComplete(task)">{{ task.completed ? '↩ Obnoviť' : '✓ Hotovo' }}</button>
          <button @click="startEdit(task)">✎ Editovať</button>
          <button @click="deleteTask(task.id)">✕ Zmazať</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container { max-width: 600px; margin: 40px auto; font-family: sans-serif; }
.form { display: flex; gap: 8px; margin-bottom: 20px; }
input { flex: 1; padding: 8px; border: 1px solid #ccc; border-radius: 4px; }
button { padding: 8px 12px; cursor: pointer; border: none; border-radius: 4px; background: #4f46e5; color: white; }
ul { list-style: none; padding: 0; }
li { padding: 12px; border: 1px solid #eee; border-radius: 4px; margin-bottom: 8px; display: flex; flex-direction: column; gap: 4px; }
.done { text-decoration: line-through; color: #999; }
small { color: #666; }
</style>
