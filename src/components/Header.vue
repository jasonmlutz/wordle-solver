<script setup>
import {ref, inject} from "vue"

const showMenu = ref(true)
const initialLoad = ref(true)

function toggleMenu() {
  showMenu.value = !showMenu.value
  initialLoad.value = false
}

const {settings} = inject("settings")

</script>

<template>
  <div
    id="header"
    class="pt-2 md:pt-4 flex flex-col items-center relative w-[300px] mx-auto"
  >
    <span class="text-xl">Wordle Solver</span>
    <div
      id="settingsIcon"
      class="text-2xl absolute inset-y-3 left-3 z-30"
    >
      <svg
        stroke="currentColor"
        fill="currentColor"
        stroke-width="0"
        viewBox="0 0 512 512"
        height="1em"
        width="1em"
        xmlns="http://www.w3.org/2000/svg"
        class=" hover:rotate-180 transition duration-500 origin-center"
        @click="toggleMenu"
      >
        <path
          d="M413.967 276.8c1.06-6.235 1.06-13.518 1.06-20.8s-1.06-13.518-1.06-20.8l44.667-34.318c4.26-3.118 5.319-8.317 2.13-13.518L418.215 115.6c-2.129-4.164-8.507-6.235-12.767-4.164l-53.186 20.801c-10.638-8.318-23.394-15.601-36.16-20.801l-7.448-55.117c-1.06-4.154-5.319-8.318-10.638-8.318h-85.098c-5.318 0-9.577 4.164-10.637 8.318l-8.508 55.117c-12.767 5.2-24.464 12.482-36.171 20.801l-53.186-20.801c-5.319-2.071-10.638 0-12.767 4.164L49.1 187.365c-2.119 4.153-1.061 10.399 2.129 13.518L96.97 235.2c0 7.282-1.06 13.518-1.06 20.8s1.06 13.518 1.06 20.8l-44.668 34.318c-4.26 3.118-5.318 8.317-2.13 13.518L92.721 396.4c2.13 4.164 8.508 6.235 12.767 4.164l53.187-20.801c10.637 8.318 23.394 15.601 36.16 20.801l8.508 55.117c1.069 5.2 5.318 8.318 10.637 8.318h85.098c5.319 0 9.578-4.164 10.638-8.318l8.518-55.117c12.757-5.2 24.464-12.482 36.16-20.801l53.187 20.801c5.318 2.071 10.637 0 12.767-4.164l42.549-71.765c2.129-4.153 1.06-10.399-2.13-13.518l-46.8-34.317zm-158.499 52c-41.489 0-74.46-32.235-74.46-72.8s32.971-72.8 74.46-72.8 74.461 32.235 74.461 72.8-32.972 72.8-74.461 72.8z"
        />
      </svg>
    </div>
    <div
      v-if="showMenu"
      id="settingsMenu"
      class="absolute inset-y-14 left-4 z-20"
    >
      <ul class="relative bg-gray-700 border rounded-md p-3">
        <li
          v-if="initialLoad"
          class="uppercase text-green-500 text-xl text-center"
        >
          new feature
        </li>
        <li>
          <input
            id="checkbox"
            v-model="settings.autoReload"
            type="checkbox"
          >
          <label
            for="checkbox"
            class="pl-1"
          >Update solutions automatically</label>
        </li>
        <li class=" pt-2 md:pt-4 flex flex-col items-center">
          <button
            class="bg-white border rounded-md text-black px-1 uppercase hover:bg-gray-400 uppercase"
            @click="toggleMenu"
          >
            close
          </button>
        </li>
      </ul>
    </div>
    <div
      v-if="showMenu"
      class="absolute top-0 v-10 bg-black opacity-50 h-screen w-[360px]"
      @click="toggleMenu"
    />
  </div>
</template>