<script setup>
import { ListChecks } from 'lucide-vue-next'
import Task from './Task.vue'

defineProps({
  tasks: Array,
  isBackgroundColorLight: Boolean,
})

const emit = defineEmits(['removeTask', 'openUpdate', 'updateComplete'])
</script>

<template>
  <div class="flex flex-col items-center w-full px-4 pb-4 overflow-y-auto">
    <TransitionGroup
      name="task"
      tag="div"
      class="flex flex-col gap-2 w-full max-w-2xl"
    >
      <Task
        v-for="(task, index) in tasks"
        :key="task.id"
        :title="task.title"
        :isComplete="task.isComplete"
        :index="index"
        :is-background-color-light="isBackgroundColorLight"
        @onClickRemoveTask="$emit('removeTask', task)"
        @onClickOpenUpdateModal="$emit('openUpdate', task)"
        @onUpdateComplete="$emit('updateComplete', task)"
      />
    </TransitionGroup>

    <div
      v-if="tasks.length === 0"
      class="flex flex-col items-center justify-center mt-16 gap-4"
    >
      <ListChecks
        :class="[
          'w-16 h-16',
          isBackgroundColorLight ? 'text-gray-300' : 'text-slate-600',
        ]"
      />
      <p
        :class="[
          'text-lg font-medium',
          isBackgroundColorLight ? 'text-gray-400' : 'text-slate-500',
        ]"
      >
        No tasks yet
      </p>
    </div>
  </div>
</template>
