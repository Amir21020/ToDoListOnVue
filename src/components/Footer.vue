<script setup>

const emit = defineEmits(['openAddTaskModal'])
</script>


<template>
  <div class="flex justify-end mt-auto  mr-96  items-end">
      <button @click="$emit('openAddTaskModal')">
        <img src="/Add button.svg" alt="addTask">
      </button>
    </div>
</template>
