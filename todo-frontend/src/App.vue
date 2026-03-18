<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { CheckCircle, Pencil, Trash2, Lightbulb, RotateCcw, Clock } from 'lucide-vue-next'

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

function formatDateTime(dateStr) {
  if (!dateStr) return ''
  const d = new Date(dateStr)
  return d.toLocaleDateString('sk-SK', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' })
}

onMounted(fetchTasks)
</script>

<template>
  <div class="min-h-screen px-4 py-10" style="background-color: #2c2c2c">
    <div class="max-w-lg mx-auto">


      <div class="flex items-center justify-between mb-10">
        <div class="flex items-center gap-2">
          <Lightbulb class="w-5 h-5" style="color: #0072BC" />
          <span class="text-base font-semibold text-white">Správca úloh</span>
        </div>
      </div>


      <div class="rounded-2xl p-5 mb-8" style="background-color: #383838">
        <input
          v-model="newTitle"
          placeholder="Názov úlohy"
          class="w-full border-b px-0 py-2 mb-3 text-sm text-white placeholder-gray-500 focus:outline-none bg-transparent"
          style="border-color: #4a4a4a"
        />
        <input
          v-model="newDescription"
          placeholder="Popis"
          class="w-full border-b px-0 py-2 mb-5 text-sm text-white placeholder-gray-500 focus:outline-none bg-transparent"
          style="border-color: #4a4a4a"
        />
        <button
          @click="addTask"
          class="text-sm font-medium text-white px-5 py-2 rounded-full transition hover:opacity-80"
          style="background-color: #0072BC"
        >
          + Pridať
        </button>
      </div>


      <div v-if="tasks.length === 0" class="text-center py-16 text-sm" style="color: #555">
        Žiadne úlohy
      </div>


      <div
        v-for="task in tasks"
        :key="task.id"
        class="rounded-2xl p-4 mb-3 border transition"
        :style="task.completed
          ? 'background-color: #323232; border-color: #3a3a3a'
          : 'background-color: #383838; border-color: #0072BC33'"
      >
  
        <div v-if="editingTask && editingTask.id === task.id">
          <input
            v-model="editingTask.title"
            class="w-full border-b px-0 py-1 mb-2 text-sm text-white focus:outline-none bg-transparent"
            style="border-color: #4a4a4a"
          />
          <input
            v-model="editingTask.description"
            class="w-full border-b px-0 py-1 mb-4 text-sm text-white focus:outline-none bg-transparent"
            style="border-color: #4a4a4a"
          />
          <div class="flex gap-2">
            <button @click="saveEdit" class="text-xs text-white px-4 py-1.5 rounded-full transition hover:opacity-80" style="background-color: #22c55e">Uložiť</button>
            <button @click="editingTask = null" class="text-xs text-gray-300 px-4 py-1.5 rounded-full transition hover:opacity-80" style="background-color: #444">Zrušiť</button>
          </div>
        </div>

        <div v-else class="flex items-start justify-between gap-3">
          <div class="flex-1 min-w-0">
            <p
                class="text-sm font-medium mb-0.5 transition-colors"
                :class="task.completed ? 'line-through text-[#555]' : 'text-white'"
                style="word-wrap: break-word; overflow-wrap: anywhere;"
              >
                {{ task.title }}
              </p>
              <p v-if="task.description" 
                class="text-xs text-[#888] mb-1.5"
                style="word-wrap: break-word; overflow-wrap: anywhere;"
              >
                {{ task.description }}
              </p>
            <div class="flex items-center gap-1 mt-1.5 text-xs" style="color: #555">
              <Clock class="w-3 h-3" />
              <span>{{ formatDateTime(task.created_at) }}</span>
            </div>
          </div>

          <div class="flex items-center gap-1 shrink-0">
            <button @click="toggleComplete(task)" class="group p-1.5 rounded-lg transition" style="hover:background-color:#2c2c2c">
              <CheckCircle v-if="!task.completed" class="w-5 h-5 transition" style="color: #fff" onmouseover="this.style.color='#22c55e'" onmouseout="this.style.color='#fff'" />
              <RotateCcw v-else class="w-5 h-5 transition" style="color: #444" onmouseover="this.style.color='#22c55e'" onmouseout="this.style.color='#fff'" />
            </button>
            <button @click="startEdit(task)" class="group p-1.5 rounded-lg transition">
              <Pencil class="w-5 h-5 transition" style="color: #fff" onmouseover="this.style.color='#eab308'" onmouseout="this.style.color='#fff'" />
            </button>
            <button @click="deleteTask(task.id)" class="group p-1.5 rounded-lg transition">
              <Trash2 class="w-5 h-5 transition" style="color: #fff" onmouseover="this.style.color='#ef4444'" onmouseout="this.style.color='#fff'" />
            </button>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>
