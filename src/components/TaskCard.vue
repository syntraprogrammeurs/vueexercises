<script setup>
import UserBadge from "./UserBadge.vue"

const props = defineProps({
  title: { type: String, required: true },
  description: { type: String, default: "" },
  assignee: { type: String, required: true },
  role: { type: String, required: true },
  priority: { type: String, default: "" }
})
</script>

<template>
  <article class="rounded-lg bg-gray-800 p-4 shadow-md border border-gray-700">
    <div class="flex items-start justify-between">
      <h3
          class="font-semibold text-white"
          :class="props.description ? 'text-sm' : 'text-base'"
      >
        {{ props.title }}
      </h3>

      <span
          v-if="props.priority"
          class="ml-2 rounded-full px-2 py-0.5 text-[10px] font-medium border"
          :class="
          props.priority === 'hoog'
            ? 'bg-red-500/20 border-red-400/70 text-red-100'
            : props.priority === 'middel'
              ? 'bg-yellow-500/20 border-yellow-400/70 text-yellow-100'
              : 'bg-gray-600/70 border-gray-500/70 text-gray-100'
        "
      >
        {{ props.priority }}
      </span>
    </div>

    <p v-if="props.description" class="text-xs text-gray-300 mt-1 leading-relaxed">
      {{ props.description }}
    </p>

    <div class="flex justify-between items-center mt-4">
      <UserBadge :label="props.role" type="role" />
      <UserBadge :label="props.assignee" type="assignee" />
    </div>
  </article>
</template>
