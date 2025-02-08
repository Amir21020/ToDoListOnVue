<script setup>

defineProps({
  isBackgroundColorLight : Boolean,
})

const emit  = defineEmits(['onSwitchBackgroundColor', 'onChangeSelect', 'onChangeSearchInput'])


</script>

<template>
  <div class="flex flex-col mt-5 w-screen justify-start ">
    <h2 :class="['font-bold', 'text-center',
    isBackgroundColorLight ? 'text-black' : 'text-white'
    ]">
      TODO LIST
    </h2>
    <div class="flex flex-row gap-3 justify-center">
      <form class="w-lg">
          <div class="flex">
              <div class="relative w-full">
                <input
                  type="text"
                  @change="event => $emit('onChangeSearchInput', event)"
                  id="search-dropdown"
                  :class="['block', 'p-2.5', 'w-full', 'text-lg',
                  'rounded-lg', 'border-2', 'focus:border-blue-500',
                  isBackgroundColorLight ? ['border-indigo-500','bg-white', 'text-purple-950'] : ['border-white', 'bg-black', 'text-white']]"
                  placeholder="Search note..." />
                  <button class="absolute right-4 top-1 mt-2 mr-2" type="button">
                      <img :src="isBackgroundColorLight ?  '/Vector (2).svg' : '/Vector (1).svg'" alt="Search icon" class="h-6 w-6">
                  </button>
              </div>
          </div>
      </form>
      <select @change="event => $emit('onChangeSelect', event)" class="border-1 border-indigo-500 appearance-auto cursor-pointer  text-white bg-indigo-500 rounded-md transition-colors duration-300 focus:border-indigo-500 hover:ring-indigo-500">
        <option class="bg-white text-indigo-500" value="">ALL</option>
        <option class="bg-white text-indigo-500" value="true">Complete</option>
        <option class="bg-white text-indigo-500" value="false">Incomplete</option>
      </select>
      <button @click="$emit('onSwitchBackgroundColor')">
        <img :src="isBackgroundColorLight ? '/Color Scheme.svg' : '/Color Scheme (1).svg'" alt="backgroundChange" class="size-12">
      </button>
    </div>
  </div>
</template>
