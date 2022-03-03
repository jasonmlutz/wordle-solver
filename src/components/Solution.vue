<script setup>
import { inject, computed } from "vue"
import {words} from "../resources/Words"

const props = defineProps({
  toggleKeyboard: {
    type: Function,
  },
  showKeyboard: {
    type: Boolean,
    default: false
  }
})

const {store} = inject("store")

function wordContainsAbsentLetter(word) {
  // returns TRUE if any of the absent letters are in the word
  const absentLetters = store.value.filter(el => el.flag === 0).map(el => el.letter)
  return absentLetters.some(letter => word.toUpperCase().includes(letter.toUpperCase()))
}

function wordOmitsRequiredLetter(word) {
  // returns TRUE if the word fails to contain a single required letter in the proper position
  const correctLetterData = store.value.filter(el => el.flag === 2)
  return correctLetterData.some(data => word.toUpperCase()[data.position] !== data.letter.toUpperCase())
}

function wordViolatesIncorrectLetter(word) {
  // returns TRUE if the word contains a present letter in an incorrect position
  // or fails to contain a present letter
  const incorrectLetterData = store.value.filter(el => el.flag === 1)
  return incorrectLetterData.some(data => (
    (word.toUpperCase()[data.position] === data.letter.toUpperCase()) ||
    !word.toUpperCase().includes(data.letter.toUpperCase())
  ))
}

const filteredWords= computed(() => {
  // build new array of those words for which the test returns TRUE
  // that is, remove words which DO have a problem
  return words.filter(word => !(wordContainsAbsentLetter(word) || wordOmitsRequiredLetter(word) || wordViolatesIncorrectLetter(word)))
})

// correct
const correctLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 2)
    .map(el => el.letter.toUpperCase())
  return [...new Set(letters)].sort()
})

// incorrect
const incorrectLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 1)
    .map(el => el.letter.toUpperCase())
    // .filter(letter => (
    //   !correctLetters.value.includes(letter)
    // ))
  return [...new Set(letters)].sort()
})

// absent
const absentLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 0)
    .map(el => el.letter.toUpperCase())
    // .filter(letter => (
    //   !correctLetters.value.includes(letter) &&
    //   !incorrectLetters.value.includes(letter)
    // ))
  return [...new Set(letters)].sort()
})

const buttonMessage = computed(() => {
  return props.showKeyboard ? "SHOW" : "HIDE"
})


</script>

<template>
  <!-- <ul
    class="pt-4"
  >
    <li>
      Letters not in answer: {{ absentLetters.join(", ") }}
    </li>
    <li>
      Letters in answer, unknown position: {{ incorrectLetters.join(", ") }}
    </li>
    <li>
      Letters in answer, known position: {{ correctLetters.join(", ") }}
    </li>
  </ul> -->
  <div
    class="pt-2 md:pt-4"
  >
    Potential Solutions: {{ filteredWords.length }}
    <button
      class="bg-white border rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400 mt-4 uppercase"
      @click="toggleKeyboard"
    >
      {{ buttonMessage }}
    </button>
  </div>
  <div
    v-if="filteredWords && filteredWords.length < 500 && !showKeyboard"
    class="pt-2 px-3 mt-2 uppercase bg-gray-800 h-[150px] w-[350px] overflow-y-auto"
  >
    {{ filteredWords.join(", ") }}
  </div>
  <div
    v-if="filteredWords.length >= 500 && !showKeyboard"
    class="pt-2 md:pt-4"
  >
    Solutions will be displayed when there are fewer than 500 options.
  </div>
  <!-- <button
    v-if="
    !showKeyboard"
    class="bg-white border rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400 mt-4 uppercase"
    @click="toggleKeyboard"
  >
    show keyboard
    </button> -->
</template>