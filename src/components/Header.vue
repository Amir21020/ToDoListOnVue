<script setup>
import { Search, Moon, Sun } from 'lucide-vue-next'

defineProps({
  isBackgroundColorLight: Boolean,
})

defineEmits(['onSwitchBackgroundColor', 'onChangeSelect', 'onChangeSearchInput'])
</script>

<template>
  <header class="w-full px-4 sm:px-8 pt-6 pb-4">
    <h1
      :class="[
        'text-3xl sm:text-4xl font-bold text-center tracking-tight mb-6',
        isBackgroundColorLight ? 'text-gray-900' : 'text-white',
      ]"
    >
      TODO LIST
    </h1>
    <div class="flex flex-col sm:flex-row items-center gap-3 max-w-2xl mx-auto">
      <div class="relative w-full">
        <input
          type="text"
          @input="event => $emit('onChangeSearchInput', event)"
          :class="[
            'w-full pl-10 pr-4 py-2.5 text-base rounded-lg border-2 outline-none transition-colors duration-200',
            isBackgroundColorLight
              ? 'border-indigo-400 bg-white text-gray-900 placeholder-gray-400 focus:border-indigo-600'
              : 'border-indigo-500 bg-slate-800 text-white placeholder-gray-500 focus:border-indigo-400',
          ]"
          placeholder="Search notes..."
        />
        <Search
          :class="[
            'absolute left-3 top-1/2 -translate-y-1/2 w-5 h-5',
            isBackgroundColorLight ? 'text-gray-400' : 'text-gray-500',
          ]"
        />
      </div>

      <div class="flex items-center gap-2 w-full sm:w-auto">
        <select
          @change="event => $emit('onChangeSelect', event)"
          :class="[
            'flex-1 sm:flex-none px-3 py-2.5 rounded-lg border-2 text-sm font-medium cursor-pointer outline-none transition-colors duration-200',
            isBackgroundColorLight
              ? 'border-indigo-400 bg-indigo-500 text-white hover:bg-indigo-600'
              : 'border-indigo-500 bg-indigo-600 text-white hover:bg-indigo-700',
          ]"
        >
          <option value="" :class="isBackgroundColorLight ? 'bg-white text-indigo-600' : 'bg-slate-800 text-indigo-300'">All</option>
          <option value="true" :class="isBackgroundColorLight ? 'bg-white text-indigo-600' : 'bg-slate-800 text-indigo-300'">Done</option>
          <option value="false" :class="isBackgroundColorLight ? 'bg-white text-indigo-600' : 'bg-slate-800 text-indigo-300'">Pending</option>
        </select>

        <button
          @click="$emit('onSwitchBackgroundColor')"
          :class="[
            'p-2.5 rounded-lg border-2 transition-colors duration-200 cursor-pointer',
            isBackgroundColorLight
              ? 'border-gray-300 bg-gray-100 text-gray-700 hover:bg-gray-200'
              : 'border-slate-600 bg-slate-800 text-yellow-400 hover:bg-slate-700',
          ]"
        >
          <Moon v-if="isBackgroundColorLight" class="w-5 h-5" />
          <Sun v-else class="w-5 h-5" />
        </button>
      </div>
    </div>
  </header>
</template>
