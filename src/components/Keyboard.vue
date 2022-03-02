<script setup>

import {ref} from "vue"

const props = defineProps({
  deleteLastLetter: {
    type: Function,
  },
  handleSubmit: {
    type: Function,
  },
  toggleKeyboard: {
    type: Function,
  }
})

var firstRow = ["q", "w", "e", "r", "t", "y", "u", "i", "o", "p"]
firstRow = firstRow.map((el, index) => {
  return {letter: el, index: index}
})

var secondRow = ["a", "s", "d", "f", "g", "h", "j", "k", "l"]
secondRow = secondRow.map((el, index) => {
  return {letter: el, index: index}
})

var thirdRow = ["z", "x", "c", "v", "b", "n", "m"]
thirdRow = thirdRow.map((el, index) => {
  return {letter: el, index: index}
})

const letterStyle = ref("uppercase border rounded-md bg-gray-500 m-0.5 w-8 h-14 pt-3 text-xl text-center hover:bg-gray-700")
const buttonStyle = ref("text-s border rounded-md bg-gray-500 m-0.5 w-12 h-14 pt-4 text-center hover:bg-gray-700")

</script>

<template>
  <div
    id="keyboard"
    class="flex flex-col pt-4"
  >
    <ul
      id="keyboard-first-row"
      class="flex flex-row justify-between"
    >
      <li
        v-for="element in firstRow"
        :key="element.index"
        :class="letterStyle"
        @click="handleSubmit(element.letter)"
      >
        {{ element.letter }}
      </li>
    </ul>
    <ul
      id="keyboard-second-row"
      class="flex flex-row justify-center"
    >
      <li
        v-for="element in secondRow"
        :key="element.index"
        :class="letterStyle"
        @click="handleSubmit(element.letter)"
      >
        {{ element.letter }}
      </li>
    </ul>
    <ul
      id="keyboard-third-row"
      class="flex flex-row justify-center"
    >
      <li
        :class="buttonStyle"
        @click="toggleKeyboard"
      >
        HIDE
      </li>
      <li
        v-for="element in thirdRow"
        :key="element.index"
        :class="letterStyle"
        @click="handleSubmit(element.letter)"
      >
        {{ element.letter }}
      </li>
      <li
        :class="buttonStyle"
        @click="deleteLastLetter"
      >
        DEL
      </li>
    </ul>
  </div>
</template>