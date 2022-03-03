<script setup>

import {inject, watch, ref} from "vue"

const props = defineProps({
  deleteLastLetter: {
    type: Function,
    default: null,
  },
  handleSubmit: {
    type: Function,
    default: null,
  },
  toggleKeyboard: {
    type: Function,
    default: null,
  },
  clearAllLetters: {
    type: Function,
    default: null,
  }
})

const buttonStyle = "text-s border rounded-md bg-gray-600 m-0.5 w-12 h-14 text-center hover:bg-gray-700"

var letterStyle = "uppercase border rounded-md m-0.5 w-8 h-14 pt-3 text-xl text-center hover:bg-gray-700"

var keyboardLetters = ["q", "w", "e", "r", "t", "y", "u", "i", "o", "p"] // length 10
keyboardLetters = keyboardLetters.concat(["a", "s", "d", "f", "g", "h", "j", "k", "l"]) //9 more
keyboardLetters = keyboardLetters.concat(["z", "x", "c", "v", "b", "n", "m"]) // 7 more

const keyboardKeys = ref(keyboardLetters.map((el, index) => {
  return {letter: el, index: index, color: " bg-gray-600"}
}))

function handleClear() {
  keyboardKeys.value = keyboardKeys.value.map(key => {
    return {...key, color: " bg-gray-600"}
  })
  props.clearAllLetters()
}

const { store } = inject("store");

watch(store.value, () => {
  keyboardKeys.value = keyboardKeys.value.map(key => {
    var reversedStore = [...store.value].reverse()
    var letterData = reversedStore.find(data => data.letter.toUpperCase() === key.letter.toUpperCase())
    letterData = letterData || {}
    var letterColor
    switch (letterData.flag) {
    case 0:
      letterColor = " bg-gray-600"
      break;
    case 1:
      letterColor = " bg-yellow-500"
      break;
    case 2:
      letterColor = " bg-green-800"
      break;
    default:
      letterColor = " bg-gray-600"
      break;
    }

    return {...key, color: letterColor}
  })
})
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
        v-for="element in keyboardKeys.slice(0,10)"
        :key="element.index"
        :class="letterStyle + element.color"
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
        v-for="element in keyboardKeys.slice(10,19)"
        :key="element.index"
        :class="letterStyle + element.color"
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
        :class="buttonStyle + ` text-xs  pt-5`"
        @click="handleClear"
      >
        CLEAR
      </li>
      <li
        v-for="element in keyboardKeys.slice(19, 26)"
        :key="element.index"
        :class="letterStyle + element.color"
        @click="handleSubmit(element.letter)"
      >
        {{ element.letter }}
      </li>
      <li
        :class="buttonStyle + ` pt-4`"
        @click="deleteLastLetter"
      >
        DEL
      </li>
    </ul>
  </div>
</template>