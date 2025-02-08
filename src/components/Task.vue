<script setup>


defineProps({
  id: Number,
  index: Number,
  isComplete: Boolean,
  title: String,
  isBackgroundColorLight: Boolean,
})

const emit = defineEmits(['onClickRemoveTask', 'onClickOpenUpdateModal', 'onUpdateComplete'])


</script>

<template>
  <div :class="['flex', 'flex-row', 'mb-2', 'border-indigo-500', index !== 0 ? ['border-t-2'] : '']">
    <div class="flex items-center ml-2">
      <label :for="'checked-checkbox-' + id" :class="[
            'ms-3',
            'flex items-center',
            'text-4xl',
            'font-medium',
            isBackgroundColorLight ? 'text-gray-900' : 'text-white'
        ]">
        <input
          @change="$emit('onUpdateComplete')"
          :id="'checked-checkbox-' + id"
          type="checkbox"
          :checked="isComplete"
          class="size-6 bg-gray-100 border-4 border-indigo-500 rounded-lg focus:ring-indigo-500 accent-indigo-500"
        />
        <span :class="isComplete ? 'line-through ml-2' : 'ml-2'">{{ title }}</span>
      </label>
    </div>
    <div class="flex flex-row ml-80 gap-3">
      <button @click="$emit('onClickOpenUpdateModal')">
        <img src="/Vector.svg" alt="updateTask" class="w-4 h-4">
      </button>
      <button @click="$emit('onClickRemoveTask')">
        <img src="/trash-svgrepo-com 1.svg" alt="deleteTask" class="w-4 h-4">
      </button>
    </div>
  </div>
</template>
