<script setup>
import { ref, onMounted } from 'vue'
import { X, Check, FileEdit, FilePlus } from 'lucide-vue-next'

const emit = defineEmits(['closeAddButton', 'addToTaskList', 'updateToTaskList'])

const props = defineProps({
  isAdded: Boolean,
  isBackgroundColorLight: Boolean,
  title: String,
})

const inputValue = ref(null)

onMounted(() => {
  if (!props.isAdded) {
    inputValue.value = props.title || ''
  }
})

const handleTaskListAction = (actionType) => {
  if (inputValue.value && inputValue.value.trim() !== '') {
    emit(actionType, { title: inputValue.value, isComplete: false })
    inputValue.value = ''
    emit('closeAddButton')
  }
}

const addToTaskList = () => handleTaskListAction('addToTaskList')
const updateToTaskList = () => handleTaskListAction('updateToTaskList')

const handleKeyword = (event) => {
  if (event.key === 'Enter') {
    props.isAdded ? addToTaskList() : updateToTaskList()
  }
}
</script>

<template>
  <div class="fixed inset-0 z-50 flex items-center justify-center p-4">
      <div
        class="absolute inset-0 bg-black/60 backdrop-blur-sm"
        @click="$emit('closeAddButton')"
      ></div>

      <div
        :class="[
          'modal-content relative w-full max-w-md rounded-xl shadow-2xl p-6 transition-colors duration-200',
          isBackgroundColorLight ? 'bg-white' : 'bg-slate-800',
        ]"
      >
        <div class="flex items-center gap-3 mb-6">
          <FileEdit v-if="!isAdded" class="w-6 h-6 text-indigo-500" />
          <FilePlus v-else class="w-6 h-6 text-indigo-500" />
          <h2
            :class="[
              'text-xl font-bold',
              isBackgroundColorLight ? 'text-gray-900' : 'text-white',
            ]"
          >
            {{ isAdded ? 'New Note' : 'Edit Note' }}
          </h2>
        </div>

        <input
          type="text"
          v-model="inputValue"
          :class="[
            'w-full px-4 py-3 rounded-lg border-2 outline-none text-base transition-colors duration-200',
            isBackgroundColorLight
              ? 'border-gray-200 bg-gray-50 text-gray-900 placeholder-gray-400 focus:border-indigo-500 focus:bg-white'
              : 'border-slate-600 bg-slate-700 text-white placeholder-gray-500 focus:border-indigo-400',
          ]"
          placeholder="What needs to be done?"
          @keydown="handleKeyword"
        />

        <div class="flex justify-between mt-6">
          <button
            @click="$emit('closeAddButton')"
            :class="[
              'flex items-center gap-2 px-4 py-2.5 rounded-lg border-2 font-medium transition-colors duration-200 cursor-pointer',
              isBackgroundColorLight
                ? 'border-gray-200 text-gray-600 hover:bg-gray-100'
                : 'border-slate-600 text-slate-300 hover:bg-slate-700',
            ]"
          >
            <X class="w-4 h-4" />
            Cancel
          </button>
          <button
            @click="isAdded ? addToTaskList() : updateToTaskList()"
            :disabled="!inputValue || inputValue.trim() === ''"
            :class="[
              'flex items-center gap-2 px-5 py-2.5 rounded-lg font-medium transition-colors duration-200 cursor-pointer',
              (!inputValue || inputValue.trim() === '')
                ? 'bg-gray-400 cursor-not-allowed opacity-50'
                : 'bg-indigo-500 hover:bg-indigo-600 text-white'
            ]"
          >
            <Check class="w-4 h-4" />
            {{ isAdded ? 'Add' : 'Save' }}
          </button>
        </div>
    </div>
  </div>
</template>
