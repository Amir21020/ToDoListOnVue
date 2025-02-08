<script setup>
import { ref } from 'vue'

const emit = defineEmits(['closeAddButton', 'addToTaskList', 'updateToTaskList'])

const props = defineProps({
  isAdded: Boolean,
  isBackgroundColorLight: Boolean,
})

const inputValue = ref(null)

const handleTaskListAction = (actionType) => {
  if (inputValue.value && inputValue.value.trim() !== '') {
    emit(actionType, { title: inputValue.value, isComplete : false });
    inputValue.value = '';
    emit('closeAddButton');
  } else {
    alert('Введите название задачи');
  }
};

const addToTaskList = () => {
  handleTaskListAction('addToTaskList');
};

const updateToTaskList = () => {
  handleTaskListAction('updateToTaskList');
};

const handleKeyword = (event) => {
  if (event.key === 'Enter') {
    props.isAdded === true ? addToTaskList() : updateToTaskList()
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div :class="['flex', 'flex-col', 'fixed', 'pt-2', 'z-20', 'rounded-xl', 'bottom-2/5', 'top-1/6', 'right-1/3', 'left-1/3', 'h-80',
    isBackgroundColorLight ? 'bg-white' : 'bg-black'
  ]">
    <h2 :class="['whitespace-nowrap', 'text-center',
      isBackgroundColorLight ? 'text-black' : 'text-white'
    ]" v-text="isAdded ? 'NEW NOTE' : 'UPDATE NOTE'"></h2>
    <div class="justify-center flex">
      <input type="text" v-model="inputValue" :class="['border-2', 'px-2', 'rounded-sm', 'w-5/6', 'h-9',
        isBackgroundColorLight ? ['text-indigo-500', 'border-indigo-500'] : ['text-white', 'border-white']
      ]" placeholder="Input your note..."
      @keydown="handleKeyword">
    </div>
    <div class="mt-auto mb-3 mx-5 flex justify-between items-end">
      <button @click="$emit('closeAddButton')">
        <img :src="isBackgroundColorLight ? '/Select (1).svg' : '/Select (2).svg'" alt="cancel">
      </button>
      <button @click="isAdded ? addToTaskList() : updateToTaskList()">
        <img src="/Select.svg" alt="apply">
      </button>
    </div>
  </div>
</template>
