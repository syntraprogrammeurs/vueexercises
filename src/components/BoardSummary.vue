<script setup>
const props = defineProps({
  columns: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section
      class="mb-6 bg-slate-900 border border-slate-800 rounded-xl p-4 text-sm"
  >
    <h2 class="text-sm font-semibold mb-3">
      Overzicht van de kolommen
    </h2>

    <div class="grid grid-cols-3 gap-3 text-xs">
      <article
          v-for="column in props.columns"
          :key="column.id"
          class="rounded-lg border border-slate-700 bg-slate-950/60 px-3 py-2"
      >
        <p class="font-semibold text-slate-100">
          {{ column.title }}
        </p>
        <p class="mt-1 text-slate-400">
          Aantal taken:
          <span class="font-semibold text-slate-100">
            {{ column.tasks.length }}
          </span>
        </p>
      </article>
    </div>
  </section>
</template>
