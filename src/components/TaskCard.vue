<script setup>
import UserBadge from "./UserBadge.vue"

const props = defineProps({
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
    required: true
  },
  role: {
    type: String,
    required: true
  }
})
</script>

<template>
  <article
      class="rounded-lg bg-gray-800 p-4 shadow-md border border-gray-700 flex flex-col justify-between"
  >
    <div>
      <h3 class="font-semibold text-sm text-white">
        {{ props.title }}
      </h3>

      <p
          v-if="props.description"
          class="text-xs text-gray-300 mt-1 leading-relaxed"
      >
        {{ props.description }}
      </p>
    </div>

    <!-- badges onderaan -->
    <div class="flex justify-between items-center mt-4">
      <!-- links: functie -->
      <UserBadge :label="props.role" type="role" />

      <!-- rechts: naam -->
      <UserBadge :label="props.assignee" type="assignee" />
    </div>
  </article>
</template>
