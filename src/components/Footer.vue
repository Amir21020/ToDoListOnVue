<script setup>
import { Plus } from 'lucide-vue-next'

defineEmits(['openAddTaskModal'])
</script>

<template>
  <div class="flex justify-center px-8 py-6">
    <button
      @click="$emit('openAddTaskModal')"
      class="flex items-center gap-2 px-6 py-3 rounded-lg bg-indigo-500 text-white font-medium shadow-lg hover:bg-indigo-600 hover:shadow-xl active:scale-95 transition-all duration-200 cursor-pointer"
    >
      <Plus class="w-5 h-5" />
      <span>Add Task</span>
    </button>
  </div>
</template>
