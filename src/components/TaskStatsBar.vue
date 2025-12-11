<script setup>
const props = defineProps({
  columns: {
    type: Array,
    required: true
  }
})

// Eenvoudige helperfunctie om het totaal aantal taken te tellen
function getTotalTasks() {
  let sum = 0

  for (const column of props.columns) {
    sum += column.tasks.length
  }

  return sum
}
</script>

<template>
  <section
      class="mb-4 bg-slate-900 border border-slate-800 rounded-xl p-4 text-xs"
  >
    <h2 class="text-sm font-semibold mb-2">
      Overzicht van taken
    </h2>

    <p class="mb-2 text-slate-300">
      Totaal aantal taken:
      <span class="font-bold text-slate-50">
        {{ getTotalTasks() }}
      </span>
    </p>

    <div class="flex flex-wrap gap-2">
      <span
          v-for="column in props.columns"
          :key="column.id"
          class="inline-flex items-center gap-1 rounded-full border border-slate-700 px-3 py-1"
      >
        <span class="font-semibold uppercase tracking-wide">
          {{ column.title }}
        </span>
        <span class="text-slate-300">
          ({{ column.tasks.length }})
        </span>
      </span>
    </div>
  </section>
</template>
