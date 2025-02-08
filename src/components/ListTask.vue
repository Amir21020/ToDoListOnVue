<script setup>

import Task from './Task.vue';

defineProps({
  removeTaskHandler : Function,
  openUpdateModal : Function,
  onUpdateComplete : Function,
  tasks : Array,
  isBackgroundColorLight : Boolean,
})

</script>

<template>
  <div class="flex flex-col mt-5 items-center justify-start">
    <Task  v-for="(task, index) in tasks"
    :key="task.id"
    :title="task.title"
    :isComplete="task.isComplete"
    :index="index"
    :is-background-color-light="isBackgroundColorLight"
    @onClickRemoveTask="removeTaskHandler(task)"
    @onClickOpenUpdateModal="openUpdateModal(task)"
    @onUpdateComplete="onUpdateComplete(task)"
    />
  </div>
</template>
