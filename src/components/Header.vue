<template>
  <div class="flex flex-col mt-5 w-screen justify-start ">
    <h2 class="font-bold text-center">
      TODO LIST
    </h2>
    <div class="flex flex-row gap-3 justify-center">
      <form class="w-lg">
          <div class="flex">
              <div class="relative w-full">
                  <input type="search" id="search-dropdown"
                      class="block p-2.5 w-full z-10 text-lg text-purple-950
                      bg-white rounded-lg border border-purple-500
                      focus:ring-blue-500 focus:border-blue-500" placeholder="Search note..." required />
                  <button class="absolute right-4 top-1 mt-2 mr-2 z-10" type="submit">
                      <img src="/Vector (2).svg" alt="Search icon" class="h-5 w-5">
                  </button>
              </div>
          </div>
      </form>
      <select class="border border-purple-700 appearance-auto cursor-pointer  text-white bg-indigo-500 rounded-md transition-colors duration-300 focus:border-indigo-500 hover:ring-indigo-500">
        <option class="bg-white text-indigo-500" value="">ALL</option>
        <option class="bg-white text-indigo-500" value="true">Complete</option>
        <option class="bg-white text-indigo-500" value="false">Incomplete</option>
      </select>
      <button>
        <img src="/Color Scheme.svg" alt="">
      </button>
    </div>
  </div>
</template>
