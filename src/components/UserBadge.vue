<script setup>
const props = defineProps({
  label: {
    type: String,
    required: true
  },
  // type bepaalt enkel de kleur: "role" of "assignee"
  type: {
    type: String,
    default: "role"
  }
})
</script>

<template>
  <span
      class="inline-flex items-center rounded-full px-2.5 py-1 text-[11px] font-medium border shadow-sm"
      :class="props.type === 'role'
      ? 'bg-emerald-500/15 border-emerald-400/60 text-emerald-100'
      : 'bg-indigo-500/15 border-indigo-400/60 text-indigo-100'"
  >
    {{ props.label }}
  </span>
</template>
