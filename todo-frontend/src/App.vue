<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { CheckCircle, Pencil, Trash2, Lightbulb, RotateCcw, Calendar, Clock } from 'lucide-vue-next'

const api = axios.create({ baseURL: 'http://localhost:8000/api' })

const tasks = ref([])
const newTitle = ref('')
const newDescription = ref('')
const newDueDate = ref('')
const editingTask = ref(null)

async function fetchTasks() {
  const res = await api.get('/tasks')
  tasks.value = res.data
}

async function addTask() {
  if (!newTitle.value.trim()) return
  await api.post('/tasks', {
    title: newTitle.value,
    description: newDescription.value,
    due_date: newDueDate.value || null
  })
  newTitle.value = ''
  newDescription.value = ''
  newDueDate.value = ''
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

function formatDate(dateStr) {
  if (!dateStr) return ''
  const d = new Date(dateStr)
  return d.toLocaleDateString('sk-SK', { day: '2-digit', month: '2-digit', year: 'numeric' })
}

function formatDateTime(dateStr) {
  if (!dateStr) return ''
  const d = new Date(dateStr)
  return d.toLocaleDateString('sk-SK', { day: '2-digit', month: '2-digit', year: 'numeric', hour: '2-digit', minute: '2-digit' })
}

function isOverdue(due_date) {
  if (!due_date) return false
  return new Date(due_date) < new Date()
}

onMounted(fetchTasks)
</script>

<template>
  <div class="min-h-screen bg-gray-50">

    <nav class="bg-white shadow-sm px-6 py-3 flex items-center justify-between">
      <div class="flex items-center gap-2">
        <Lightbulb class="w-6 h-6" style="color: #0072BC" />
        <span class="font-bold text-gray-700 text-lg">Správca úloh</span>
      </div>
    </nav>

    <div class="max-w-xl mx-auto py-10 px-4">

      <div class="flex flex-col items-center mb-8">
        <Lightbulb class="w-12 h-12 mb-2" style="color: #0072BC" />
        <h1 class="text-3xl font-bold text-gray-800">Správca úloh</h1>
        <p class="text-gray-400 text-sm mt-1">Pridaj, uprav alebo dokonči svoje úlohy</p>
      </div>

      <div class="bg-white rounded-2xl shadow p-6 mb-6 border-t-4" style="border-color: #0072BC">
        <input
          v-model="newTitle"
          placeholder="Názov úlohy"
          class="w-full border border-gray-300 rounded-lg px-4 py-2 mb-3 focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <input
          v-model="newDescription"
          placeholder="Popis (nepovinný)"
          class="w-full border border-gray-300 rounded-lg px-4 py-2 mb-3 focus:outline-none focus:ring-2 focus:ring-blue-400"
        />
        <div class="flex items-center gap-2 mb-4">
          <Calendar class="w-4 h-4 text-gray-400" />
          <input
            v-model="newDueDate"
            type="date"
            class="flex-1 border border-gray-300 rounded-lg px-4 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400 text-gray-600"
          />
        </div>
        <button
          @click="addTask"
          class="w-full text-white font-semibold py-2 rounded-lg transition hover:opacity-90"
          style="background-color: #0072BC"
        >
          Pridať úlohu
        </button>
      </div>

      <div v-if="tasks.length === 0" class="text-center text-gray-400 py-10">
        <Lightbulb class="w-10 h-10 mx-auto mb-2 text-gray-300" />
        Žiadne úlohy. Pridaj prvú!
      </div>

      <div
        v-for="task in tasks"
        :key="task.id"
        class="bg-white rounded-2xl shadow p-4 mb-4 border-l-4"
        :style="task.completed ? 'border-color: #22c55e' : isOverdue(task.due_date) ? 'border-color: #ef4444' : 'border-color: #0072BC'"
      >
        <div v-if="editingTask && editingTask.id === task.id">
          <input
            v-model="editingTask.title"
            class="w-full border border-gray-300 rounded-lg px-3 py-2 mb-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
          <input
            v-model="editingTask.description"
            class="w-full border border-gray-300 rounded-lg px-3 py-2 mb-2 focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
          <div class="flex items-center gap-2 mb-3">
            <Calendar class="w-4 h-4 text-gray-400" />
            <input
              v-model="editingTask.due_date"
              type="date"
              class="flex-1 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-400 text-gray-600"
            />
          </div>
          <div class="flex gap-2">
            <button @click="saveEdit" class="flex-1 bg-green-500 hover:bg-green-600 text-white py-1.5 rounded-lg transition">Uložiť</button>
            <button @click="editingTask = null" class="flex-1 bg-gray-200 hover:bg-gray-300 text-gray-700 py-1.5 rounded-lg transition">Zrušiť</button>
          </div>
        </div>

        <div v-else>
          <div class="flex items-start justify-between gap-2">
            <div class="flex-1 min-w-0 overflow-hidden">
              <p :class="task.aspect-ompleted ? 'line-through text-gray-400' : 'text-gray-800'" class="font-semibold text-lg break-words">{{ task.title }}</p>
              <p v-if="task.description" class="text-gray-500 text-sm mt-1 break-words">{{ task.description }}</p>

              <div class="flex flex-wrap gap-3 mt-2">
                <div class="flex items-center gap-1 text-xs text-gray-400">
                  <Clock class="w-3 h-3" />
                  <span>Vytvorené: {{ formatDateTime(task.created_at) }}</span>
                </div>
                <div v-if="task.due_date" class="flex items-center gap-1 text-xs" :class="isOverdue(task.due_date) && !task.completed ? 'text-red-500' : 'text-gray-400'">
                  <Calendar class="w-3 h-3" />
                  <span>Termín: {{ formatDate(task.due_date) }}</span>
                  <span v-if="isOverdue(task.due_date) && !task.completed" class="text-red-500 font-medium">(po termíne)</span>
                </div>
              </div>
            </div>
            <span
              :class="task.completed ? 'bg-green-100 text-green-700' : isOverdue(task.due_date) ? 'bg-red-100 text-red-600' : 'bg-blue-100'"
              :style="!task.completed && !isOverdue(task.due_date) ? 'color: #0072BC' : ''"
              class="text-xs font-medium px-2 py-1 rounded-full whitespace-nowrap"
            >
              {{ task.completed ? 'Hotovo' : isOverdue(task.due_date) ? 'Po termíne' : 'Čaká' }}
            </span>
          </div>

          <div class="flex gap-3 mt-4">
            <button
              @click="toggleComplete(task)"
              class="group flex-1 flex items-center justify-center gap-1.5 py-1.5 rounded-lg bg-gray-100 hover:bg-green-100 transition"
            >
              <CheckCircle v-if="!task.completed" class="w-4 h-4 text-gray-400 group-hover:text-green-600 transition" />
              <RotateCcw v-else class="w-4 h-4 text-gray-400 group-hover:text-green-600 transition" />
              <span class="text-xs text-gray-400 group-hover:text-green-600 transition">{{ task.completed ? 'Obnoviť' : 'Hotovo' }}</span>
            </button>

            <button
              @click="startEdit(task)"
              class="group flex-1 flex items-center justify-center gap-1.5 py-1.5 rounded-lg bg-gray-100 hover:bg-yellow-100 transition"
            >
              <Pencil class="w-4 h-4 text-gray-400 group-hover:text-yellow-600 transition" />
              <span class="text-xs text-gray-400 group-hover:text-yellow-600 transition">Editovať</span>
            </button>

            <button
              @click="deleteTask(task.id)"
              class="group flex-1 flex items-center justify-center gap-1.5 py-1.5 rounded-lg bg-gray-100 hover:bg-red-100 transition"
            >
              <Trash2 class="w-4 h-4 text-gray-400 group-hover:text-red-600 transition" />
              <span class="text-xs text-gray-400 group-hover:text-red-600 transition">Zmazať</span>
            </button>
          </div>
        </div>
      </div>

    </div>
  </div>
</template>
