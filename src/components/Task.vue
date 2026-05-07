<script setup>
import { Pencil, Trash2, Check } from 'lucide-vue-next'

defineProps({
  id: Number,
  index: Number,
  isComplete: Boolean,
  title: String,
  isBackgroundColorLight: Boolean,
})

defineEmits(['onClickRemoveTask', 'onClickOpenUpdateModal', 'onUpdateComplete'])
</script>

<template>
  <div
    :class="[
      'flex items-center justify-between w-full max-w-2xl px-4 py-3 rounded-xl transition-colors duration-200',
      isBackgroundColorLight
        ? 'bg-white hover:bg-gray-50 border border-gray-200'
        : 'bg-slate-800 hover:bg-slate-700 border border-slate-700',
    ]"
  >
    <div
      class="flex items-center gap-3 flex-1 min-w-0 cursor-pointer select-none"
      @click="$emit('onUpdateComplete')"
    >
      <div
        :class="[
          'w-5 h-5 rounded-md border-2 flex items-center justify-center transition-all duration-200 shrink-0',
          isComplete
            ? 'bg-indigo-500 border-indigo-500'
            : isBackgroundColorLight
              ? 'border-gray-300'
              : 'border-slate-500',
        ]"
      >
        <input
          @change="$emit('onUpdateComplete')"
          :id="'task-' + id"
          type="checkbox"
          :checked="isComplete"
          class="sr-only"
        />
        <Check v-if="isComplete" class="w-3 h-3 text-white stroke-[3]" />
      </div>
      <span
        :class="[
          'text-lg font-medium truncate transition-all duration-200',
          isComplete
            ? 'line-through opacity-50'
            : isBackgroundColorLight
              ? 'text-gray-900'
              : 'text-white',
        ]"
      >
        {{ title }}
      </span>
    </div>

    <div class="flex items-center gap-1 ml-4">
      <button
        @click="$emit('onClickOpenUpdateModal')"
        :class="[
          'p-2 rounded-lg transition-colors duration-200 cursor-pointer',
          isBackgroundColorLight
            ? 'text-gray-400 hover:text-indigo-600 hover:bg-indigo-50'
            : 'text-slate-400 hover:text-indigo-400 hover:bg-slate-700',
        ]"
      >
        <Pencil class="w-4 h-4" />
      </button>
      <button
        @click="$emit('onClickRemoveTask')"
        :class="[
          'p-2 rounded-lg transition-colors duration-200 cursor-pointer',
          isBackgroundColorLight
            ? 'text-gray-400 hover:text-red-600 hover:bg-red-50'
            : 'text-slate-400 hover:text-red-400 hover:bg-slate-700',
        ]"
      >
        <Trash2 class="w-4 h-4" />
      </button>
    </div>
  </div>
</template>
