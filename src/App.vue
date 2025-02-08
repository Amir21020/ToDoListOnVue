<script setup>
import Header from './components/Header.vue';
import axios from 'axios';
import TaskEditor from './components/TaskEditor.vue';
import { onMounted, ref, reactive, watch } from 'vue';
import Footer from './components/Footer.vue';
import ListTask from './components/ListTask.vue';


const isBackgroundColorLight = ref(true)
const taskList = ref([])
const isEditMode = ref(false)
const isAdded = ref(false)
const elm = ref(null)


const switchBackgroundColor = () => {
  isBackgroundColorLight.value = !isBackgroundColorLight.value
}


const openAddTaskModal = () => {
  isEditMode.value = true
  isAdded.value = true
}

const openUpdateTaskButton = (items) => {
  isEditMode.value = true
  isAdded.value = false
  elm.value = items
}

const closeModifyButton = () => {
  isEditMode.value = false
}




const addToTaskListHandler = async (item) => {
  if (!taskList.value.some(task => task.title === item.title)) {
    try {
      await axios.post('https://ceb8a8aae5110a71.mokky.dev/tasks', item )
      taskList.value.push(item)
    } catch (error) {
      console.error('Ошибка при добавлении задачи:', error)
    }
  } else {
    alert('Задача с таким названием существует')
  }
}


const updateTitleTaskHandler = async (item) => {
  const isExistTitleInTaskList = taskList.value.map(task => task.id !== elm.value.id).some(task => task.title === item.title)
  if (isExistTitleInTaskList) {
    alert('Задача с таким названием существует');
  } else {
    try {
        elm.value.title = item.title
        await axios.patch(`https://ceb8a8aae5110a71.mokky.dev/tasks/${elm.value.id}`, elm.value)
      } catch (err) {
      console.log(err);
    }
  }
}

const updateCompleteTaskHandler = async (item) => {
  try {
    item.isComplete = !item.isComplete;
    await axios.patch(`https://ceb8a8aae5110a71.mokky.dev/tasks/${item.id}`, item);
  } catch (err) {
    console.error("Ошибка при обновлении задачи:", err.response ? err.response.data : err);
  }
};







const removeTaskHandler = async (item) => {
  try{
    await axios.delete(`https://ceb8a8aae5110a71.mokky.dev/tasks/${item.id}`);
    taskList.value = taskList.value.filter(task => task.id !== item.id)
  }
  catch(err){
    console.log(err)
  }

}

const filters = reactive({
  isComplete: null,
  searchQuery: '',
})

const onChangeSelect = (event) => {
  filters.isComplete = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchItems = async () => {
  try {
    const params = {}

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const {data} =  await axios.get(`https://ceb8a8aae5110a71.mokky.dev/tasks`, {
      params,
    })

    taskList.value = filters.isComplete !== ""
      ? data.filter(obj => obj.isComplete === (filters.isComplete === "true"))
      : data.map(obj => ({ ...obj }));

  } catch (err) {
    console.log(err)
  }
}



onMounted(async () => {
  try{
    const { data }  = await axios.get(`https://ceb8a8aae5110a71.mokky.dev/tasks`)
    taskList.value = data
  }
  catch(err){
    console.log(err)
  }
})

watch(filters, fetchItems)

</script>

<template>
  <TaskEditor
  :is-added="isAdded"
  :is-background-color-light="isBackgroundColorLight"
  @closeAddButton="closeModifyButton"
  @updateToTaskList="updateTitleTaskHandler"
  @addToTaskList="addToTaskListHandler"
  v-if="isEditMode" />
  <div :class="['flex', 'flex-col', 'h-screen', 'w-full', isBackgroundColorLight ? 'bg-white' : 'bg-black']">
    <Header @onSwitchBackgroundColor="switchBackgroundColor"
     @onChangeSelect="onChangeSelect"
     @onChangeSearchInput="onChangeSearchInput"
     :isBackgroundColorLight="isBackgroundColorLight"
    />

    <div v-if="taskList.length === 0" class="flex mt-2 items-center justify-center w-full">
        <img :src=" isBackgroundColorLight ? '/LIST.svg' : '/LIST (1).svg'" alt="empty" :class=" isBackgroundColorLight ? 'h-130' : 'h-60'" >
    </div>

    <ListTask v-else v-once
      :remove-task-handler="removeTaskHandler"
      :open-update-modal="openUpdateTaskButton"
      :tasks="taskList"
      :onUpdateComplete="updateCompleteTaskHandler"
      :is-background-color-light="isBackgroundColorLight" />
    <Footer @openAddTaskModal="openAddTaskModal" />
  </div>
</template>
