<script setup>

import {inject, watch, ref, onMounted} from "vue"

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
  return {letter: el, index: index, colorIndex: 0}
}))

function handleClear() {
  keyboardKeys.value = keyboardKeys.value.map(key => {
    return {...key, colorIndex: 0}
  })
  props.clearAllLetters()
}

const { store } = inject("store");

function gatherLetterFlags() {
  // iterate over the keyboard keys to update the value
  keyboardKeys.value = keyboardKeys.value.map(keyboardKey => {
    // for that key, find all associated store entries
    var associatedStoreElements = store.value.filter(el => el.letter.toUpperCase() === keyboardKey.letter.toUpperCase())
    // gather the flags for those elements
    associatedStoreElements = associatedStoreElements.map(el => el.flag)
    // compute the max index among those store entries, or 0 if no such entries
    var newColorIndex = associatedStoreElements.length ? Math.max(...associatedStoreElements) : 0
    // return the keyboardKey with the new colorIndex
    return {...keyboardKey, colorIndex: newColorIndex}
  })
}

function renderKeyColor(colorIndex) {
  // this function will translate an input like element.colorIndex to the tailwind color string
  switch (colorIndex) {
    case 0:
      return " bg-gray-600"
    case 1:
      return " bg-yellow-500"
    case 2:
      return " bg-green-800"
    default:
      return " bg-gray-600"
  }
}

onMounted(gatherLetterFlags)

watch(store, gatherLetterFlags,
  { deep: true}
)
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
        :class="letterStyle + renderKeyColor(element.colorIndex)"
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
        :class="letterStyle + renderKeyColor(element.colorIndex)"
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
        :class="letterStyle + renderKeyColor(element.colorIndex)"
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