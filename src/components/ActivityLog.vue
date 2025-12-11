<script setup>
const props = defineProps({
  entries: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <section
      class="mb-4 bg-slate-900 border border-slate-800 rounded-xl p-4 text-xs"
  >
    <h2 class="text-sm font-semibold mb-2">
      Activiteit
    </h2>

    <p v-if="props.entries.length === 0" class="text-slate-400">
      Nog geen activiteiten.
    </p>

    <ul v-else class="space-y-1 max-h-32 overflow-y-auto">
      <!-- laatste actie bovenaan: we maken een kopie en keren die om -->
      <li
          v-for="(entry, index) in props.entries.slice().reverse()
        "
          :key="index"
          class="text-slate-200"
      >
        • {{ entry }}
      </li>
    </ul>
  </section>
</template>
