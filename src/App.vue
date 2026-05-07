<script setup>
import Header from './components/Header.vue'
import axios from 'axios'
import TaskEditor from './components/TaskEditor.vue'
import { onMounted, ref, reactive, watch } from 'vue'
import Footer from './components/Footer.vue'
import ListTask from './components/ListTask.vue'

const API_URL = 'https://ceb8a8aae5110a71.mokky.dev/tasks'

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
  elm.value = null
}

const openUpdateTaskButton = (items) => {
  isEditMode.value = true
  isAdded.value = false
  elm.value = items
}

const closeModifyButton = () => {
  isEditMode.value = false
  elm.value = null
}

const showAlert = (message) => {
  alert(message)
}

const addToTaskListHandler = async (item) => {
  if (!taskList.value.some(task => task.title === item.title)) {
    try {
      const { data } = await axios.post(API_URL, item)
      filters.searchQuery = ''
      filters.isComplete = ''
      await fetchItems()
  }  catch (error) {
      console.error('Error adding task:', error)
    }
  } else {
    showAlert('A task with this title already exists')
  }
}

const updateTitleTaskHandler = async (item) => {
  const isExistTitleInTaskList = taskList.value
    .filter(task => task.id !== elm.value.id)
    .some(task => task.title === item.title)

  if (isExistTitleInTaskList) {
    showAlert('A task with this title already exists')
  } else {
    try {
      elm.value.title = item.title
      await axios.patch(`${API_URL}/${elm.value.id}`, elm.value)
    } catch (err) {
      console.log(err)
    }
  }
}

const updateCompleteTaskHandler = async (item) => {
  try {
    item.isComplete = !item.isComplete
    await axios.patch(`${API_URL}/${item.id}`, item)
  } catch (err) {
    console.error('Error updating task:', err.response ? err.response.data : err)
  }
}

const removeTaskHandler = async (item) => {
  try {
    await axios.delete(`${API_URL}/${item.id}`)
    taskList.value = taskList.value.filter(task => task.id !== item.id)
  } catch (err) {
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

    const { data } = await axios.get(API_URL, { params })

    taskList.value =
      filters.isComplete !== ''
        ? data.filter(obj => obj.isComplete === (filters.isComplete === 'true'))
        : data.map(obj => ({ ...obj }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  try {
    const { data } = await axios.get(API_URL)
    taskList.value = data
  } catch (err) {
    console.log(err)
  }
})

watch(() => filters, fetchItems, { deep: true })
</script>

<template>
  <div
    :class="[
      'flex flex-col h-screen transition-colors duration-300',
      isBackgroundColorLight ? 'bg-gray-50' : 'bg-slate-900',
    ]"
  >
    <Header
      @onSwitchBackgroundColor="switchBackgroundColor"
      @onChangeSelect="onChangeSelect"
      @onChangeSearchInput="onChangeSearchInput"
      :isBackgroundColorLight="isBackgroundColorLight"
    />

    <ListTask
      :tasks="taskList"
      :is-background-color-light="isBackgroundColorLight"
      @removeTask="removeTaskHandler"
      @openUpdate="openUpdateTaskButton"
      @updateComplete="updateCompleteTaskHandler"
    />

    <Footer @openAddTaskModal="openAddTaskModal" />
  </div>
  
  <Teleport to="body">
    <TaskEditor
      v-if="isEditMode"
      :is-added="isAdded"
      :is-background-color-light="isBackgroundColorLight"
      :title="elm?.title"
      @closeAddButton="closeModifyButton"
      @updateToTaskList="updateTitleTaskHandler"
      @addToTaskList="addToTaskListHandler"
    />
  </Teleport>
</template>
