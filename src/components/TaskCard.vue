<script setup>
const props = defineProps({
  id: {
    type: Number,
    required: true
  },
  columnId: {
    type: String,
    required: true
  },
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    default: ""
  },
  assignee: {
    type: String,
    default: ""
  },
  role: {
    type: String,
    default: ""
  },
  priority: {
    type: String,
    default: ""
  },
  done: {
    type: Boolean,
    default: false
  },
  important: {
    type: Boolean,
    default: false
  }
})

const emit = defineEmits([
  "toggle-important",
  "duplicate-task",
  "move-up",
  "move-down",
  "delete-task"
])

function handleToggleImportant() {
  emit("toggle-important", {
    taskId: props.id,
    fromColumnId: props.columnId
  })
}

function handleDuplicateTask() {
  emit("duplicate-task", {
    taskId: props.id,
    fromColumnId: props.columnId
  })
}

function handleMoveUp() {
  emit("move-up", {
    taskId: props.id,
    fromColumnId: props.columnId
  })
}

function handleMoveDown() {
  emit("move-down", {
    taskId: props.id,
    fromColumnId: props.columnId
  })
}

function handleDeleteTask() {
  emit("delete-task", {
    taskId: props.id,
    fromColumnId: props.columnId
  })
}
</script>

<template>
  <article
      class="rounded-xl bg-slate-900 border border-slate-800 p-4 flex flex-col gap-3"
  >
    <header class="flex items-center justify-between gap-2">
      <button
          type="button"
          class="text-[11px] px-2 py-1 rounded-full border border-slate-600 bg-slate-950 hover:bg-slate-800"
          :class="important ? 'text-yellow-300' : 'text-slate-300'"
          @click="handleToggleImportant"
      >
        {{ important ? "★ Belangrijk" : "☆ Markeer belangrijk" }}
      </button>

      <span
          v-if="priority"
          class="text-[11px] px-2 py-0.5 rounded-full"
          :class="priority === 'hoog'
          ? 'bg-red-900 text-red-100'
          : priority === 'middel'
          ? 'bg-yellow-900 text-yellow-100'
          : 'bg-slate-800 text-slate-200'"
      >
        {{ priority }}
      </span>
    </header>

    <div>
      <h3 class="text-sm font-semibold text-slate-50">
        {{ title }}
      </h3>

      <p v-if="description" class="mt-1 text-xs text-slate-300">
        {{ description }}
      </p>
    </div>

    <footer class="mt-1 flex flex-wrap gap-2 items-center">
      <span
          v-if="role"
          class="text-[11px] px-2 py-0.5 rounded-full border border-emerald-700 bg-emerald-900 text-emerald-100"
      >
        {{ role }}
      </span>

      <span
          v-if="assignee"
          class="inline-flex items-center gap-1 text-[11px] px-2 py-0.5 rounded-full border border-slate-700 bg-slate-800 text-slate-100"
      >
        <span
            class="inline-flex items-center justify-center w-4 h-4 rounded-full bg-slate-700 text-[10px]"
        >
          {{ assignee.charAt(0) }}
        </span>
        {{ assignee }}
      </span>
    </footer>

    <!-- Knoppenbalk: pijltjes + dupliceren + verwijderen -->
    <div class="mt-2 flex justify-between items-center">
      <div class="flex gap-1">
        <button
            type="button"
            class="text-[11px] px-2 py-1 rounded-md bg-slate-800 hover:bg-slate-700 text-slate-100"
            @click="handleMoveUp"
        >
          ↑
        </button>
        <button
            type="button"
            class="text-[11px] px-2 py-1 rounded-md bg-slate-800 hover:bg-slate-700 text-slate-100"
            @click="handleMoveDown"
        >
          ↓
        </button>
      </div>

      <div class="flex gap-2">
        <button
            type="button"
            class="text-[11px] px-3 py-1 rounded-md bg-slate-700 hover:bg-slate-600 text-slate-100"
            @click="handleDuplicateTask"
        >
          Dupliceren
        </button>

        <button
            type="button"
            class="text-[11px] px-3 py-1 rounded-md bg-red-700 hover:bg-red-600 text-slate-50"
            @click="handleDeleteTask"
        >
          Verwijderen
        </button>
      </div>
    </div>
  </article>
</template>
