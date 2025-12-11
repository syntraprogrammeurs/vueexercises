<script setup>
import { ref } from "vue"
import TaskCard from "./components/TaskCard.vue"
import BoardColumn from "./components/BoardColumn.vue"
import TaskStatsBar from "./components/TaskStatsBar.vue"
import ActivityLog from "./components/ActivityLog.vue"

const columns = ref([
  {
    id: "original",
    title: "Taken (origineel)",
    color: "blue",
    tasks: [
      {
        id: 1,
        title: "Nieuwe cursusstructuur uitwerken",
        description: "Modules herzien en oefeningen toevoegen.",
        assignee: "Tom Vanhoutte",
        role: "Full stack developer",
        priority: "hoog",
        done: false,
        important: true
      }
    ]
  },
  {
    id: "byRole",
    title: "Taken (op functie)",
    color: "green",
    tasks: [
      {
        id: 4,
        title: "Nieuwe cursusstructuur uitwerken",
        description: "Modules herzien en oefeningen toevoegen.",
        assignee: "Tom Vanhoutte",
        role: "Full stack developer",
        priority: "hoog",
        done: false,
        important: false
      },
      {
        id: 5,
        title: "Component structuur bepalen",
        description: "Structuur van de componentenboom uitwerken.",
        assignee: "Noah Martens",
        role: "Software architect",
        priority: "middel",
        done: false,
        important: false
      }
    ]
  },
  {
    id: "backlog",
    title: "Backlog",
    color: "purple",
    tasks: [
      {
        id: 7,
        title: "Nieuwe animaties uitwerken",
        description: "Ideeën verzamelen voor micro-interacties.",
        assignee: "Emma Vermeersch",
        role: "Frontend developer",
        priority: "laag",
        done: false,
        important: false
      }
    ]
  },
  {
    id: "done",
    title: "Done",
    color: "gray",
    tasks: [
      {
        id: 8,
        title: "Vite project opgezet",
        description: "Basisconfiguratie met Vue en Tailwind.",
        assignee: "Team Dev",
        role: "Developer",
        priority: "laag",
        done: true,
        important: false
      }
    ]
  }
])

const newTaskTitle = ref("")
const newTaskDescription = ref("")
const newTaskColumnId = ref("original")

const nextTaskId = ref(100)

// logboek
const activityEntries = ref([])

function log(message) {
  activityEntries.value.push(message)
}

// checkbox voor kolom "Done"
const hideDoneColumn = ref(false)

function handleAddTask() {
  const column = columns.value.find(c => c.id === newTaskColumnId.value)
  if (!column) return

  const title = newTaskTitle.value
  const description = newTaskDescription.value

  column.tasks.push({
    id: nextTaskId.value++,
    title,
    description,
    assignee: "Onbekend",
    role: "Onbekend",
    priority: "",
    done: false,
    important: false
  })

  log(`Taak '${title}' toegevoegd aan kolom '${column.title}'.`)

  newTaskTitle.value = ""
  newTaskDescription.value = ""
}

function handleAddTaskToColumn(payload) {
  const column = columns.value.find(c => c.id === payload.columnId)
  if (!column) return

  const title = payload.title

  column.tasks.push({
    id: nextTaskId.value++,
    title,
    description: "",
    assignee: "Onbekend",
    role: "Onbekend",
    priority: "",
    done: false,
    important: false
  })

  log(`Taak '${title}' toegevoegd aan kolom '${column.title}' (via kolomformulier).`)
}

function clearDone() {
  const doneColumn = columns.value.find(c => c.id === "done")
  if (!doneColumn) return

  const count = doneColumn.tasks.length

  doneColumn.tasks = []

  log(`Alle ${count} taken in 'Done' verwijderd.`)
}

// HELPER: enkel zichtbare kolommen tonen
function getVisibleColumns() {
  const result = []

  for (const column of columns.value) {
    if (hideDoneColumn.value && column.id === "done") {
      continue
    }
    result.push(column)
  }

  return result
}

// belangrijk toggelen
function handleToggleImportant(payload) {
  const column = columns.value.find(c => c.id === payload.fromColumnId)
  if (!column) return

  const task = column.tasks.find(t => t.id === payload.taskId)
  if (!task) return

  task.important = !task.important

  log(
      `Taak '${task.title}' is nu ${
          task.important ? "belangrijk" : "niet belangrijk"
      }.`
  )
}

// taak dupliceren in dezelfde kolom
function handleDuplicateTask(payload) {
  const column = columns.value.find(c => c.id === payload.fromColumnId)
  if (!column) return

  const index = column.tasks.findIndex(t => t.id === payload.taskId)
  if (index === -1) return

  const original = column.tasks[index]

  const copy = {
    id: nextTaskId.value++,
    title: original.title + " (kopie)",
    description: original.description,
    assignee: original.assignee,
    role: original.role,
    priority: original.priority,
    done: original.done,
    important: original.important
  }

  column.tasks.splice(index + 1, 0, copy)

  log(`Taak '${original.title}' gedupliceerd in kolom '${column.title}'.`)
}

// NIEUW: taak omhoog verplaatsen binnen dezelfde kolom
function handleMoveTaskUp(payload) {
  const column = columns.value.find(c => c.id === payload.fromColumnId)
  if (!column) return

  const index = column.tasks.findIndex(t => t.id === payload.taskId)
  if (index <= 0) return

  const task = column.tasks[index]
  const previous = column.tasks[index - 1]

  column.tasks[index - 1] = task
  column.tasks[index] = previous

  log(`Taak '${task.title}' omhoog verplaatst in kolom '${column.title}'.`)
}

// NIEUW: taak omlaag verplaatsen binnen dezelfde kolom
function handleMoveTaskDown(payload) {
  const column = columns.value.find(c => c.id === payload.fromColumnId)
  if (!column) return

  const index = column.tasks.findIndex(t => t.id === payload.taskId)
  if (index === -1 || index >= column.tasks.length - 1) return

  const task = column.tasks[index]
  const next = column.tasks[index + 1]

  column.tasks[index + 1] = task
  column.tasks[index] = next

  log(`Taak '${task.title}' omlaag verplaatst in kolom '${column.title}'.`)
}
</script>

<template>
  <main class="min-h-screen bg-slate-950 text-slate-100 px-6 py-8">
    <header class="mb-6">
      <h1 class="text-2xl font-bold">
        Mijn kanban board (statische versie)
      </h1>
      <p class="text-slate-400 text-sm mt-1">
        In deze versie staan alle kaarten nog vast in de code. Later maken we dit
        dynamisch.
      </p>
    </header>

    <!-- Statistiekbalk -->
    <TaskStatsBar :columns="columns" />

    <!-- Logboek van acties -->
    <ActivityLog :entries="activityEntries" />

    <!-- Blok voor Done-filter en clear-knop -->
    <section
        class="mb-6 bg-slate-900 border border-slate-800 rounded-xl p-4 text-xs flex flex-col gap-3"
    >
      <label class="inline-flex items-center gap-2 text-slate-200">
        <input
            v-model="hideDoneColumn"
            type="checkbox"
            class="rounded border-slate-600 bg-slate-800"
        />
        Verberg kolom "Done"
      </label>

      <button
          type="button"
          class="inline-flex items-center rounded-md bg-red-700 px-3 py-1.5 text-[11px] font-semibold hover:bg-red-600"
          @click="clearDone"
      >
        Wis alle taken in Done
      </button>
    </section>

    <!-- Centraal formulier -->
    <section class="mb-6 bg-slate-900 border border-slate-800 rounded-xl p-4">
      <h2 class="text-sm font-semibold mb-3">
        Nieuwe taak toevoegen
      </h2>

      <form class="space-y-3" @submit.prevent="handleAddTask">
        <div class="space-y-1">
          <label class="block text-xs font-medium text-slate-300">
            Titel
          </label>
          <input
              v-model="newTaskTitle"
              type="text"
              class="w-full rounded-md bg-slate-800 border border-slate-700 px-3 py-1.5 text-sm"
          />
        </div>

        <div class="space-y-1">
          <label class="block text-xs font-medium text-slate-300">
            Beschrijving
          </label>
          <textarea
              v-model="newTaskDescription"
              rows="3"
              class="w-full rounded-md bg-slate-800 border border-slate-700 px-3 py-1.5 text-sm"
          ></textarea>
        </div>

        <div class="space-y-1">
          <label class="block text-xs font-medium text-slate-300">
            Kolom
          </label>
          <select
              v-model="newTaskColumnId"
              class="w-full rounded-md bg-slate-800 border border-slate-700 px-3 py-1.5 text-sm"
          >
            <option
                v-for="column in columns"
                :key="column.id"
                :value="column.id"
            >
              {{ column.title }}
            </option>
          </select>
        </div>

        <button
            type="submit"
            class="mt-2 inline-flex items-center rounded-md bg-blue-600 px-4 py-1.5 text-xs font-semibold hover:bg-blue-500 disabled:opacity-40 disabled:cursor-not-allowed"
            :disabled="newTaskTitle.trim().length === 0"
        >
          Taak toevoegen (centraal)
        </button>
      </form>
    </section>

    <!-- Kolommen -->
    <section class="grid gap-6 md:grid-cols-4">
      <BoardColumn
          v-for="column in getVisibleColumns()"
          :key="column.id"
          :id="column.id"
          :title="column.title"
          :count="column.tasks.length"
          :color="column.color"
          @add-task="handleAddTaskToColumn"
      >
        <TaskCard
            v-for="task in column.tasks"
            :key="task.id"
            :id="task.id"
            :column-id="column.id"
            :title="task.title"
            :description="task.description"
            :assignee="task.assignee"
            :role="task.role"
            :priority="task.priority"
            :done="task.done"
            :important="task.important"
            @toggle-important="handleToggleImportant"
            @duplicate-task="handleDuplicateTask"
            @move-up="handleMoveTaskUp"
            @move-down="handleMoveTaskDown"
        />
      </BoardColumn>
    </section>
  </main>
</template>
