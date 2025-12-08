<script setup>
import UserBadge from "./UserBadge.vue"

const props = defineProps({
  role: { type: String, required: true },
  assignee: { type: String, required: true }
})
</script>

<template>
  <div class="flex justify-between items-center mt-4">
    <UserBadge :label="props.role" type="role" />
    <UserBadge :label="props.assignee" type="assignee" />
  </div>
</template>
