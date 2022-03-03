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
// store is be an array of the form 
// store[index] = {letter, position, flag}

function wordAvoidsAbsentLetters(word) {
  // grab all store objects corresponding to letters NOT present in the word
  var absentLetters = store.value.filter(el => el.flag === 0)
  // map to just the letters; drop the position and flag
  absentLetters = absentLetters.map(el => el.letter)

  // check whether the word contains any of the absent letters
  // returns TRUE if the test is TRUE for every element of absentLetters
  // i.e. 
  // returns TRUE if and only if the word FAILS to contain an absent letter
  return absentLetters.every(letter => !word.toUpperCase().includes(letter.toUpperCase()))
}

function wordCorrectlyContainsRequiredLetter(word) {
  // grab all store objects corresponding to required letters
  var correctLetterData = store.value.filter(el => el.flag === 2)
  // map to just {letter, position}; drop the flag
  correctLetterData = correctLetterData.map( el => {
    return {letter: el.letter, position: el.position}
  })

  // check whether the word contains each required letter in each required position
  // returns TRUE if and only if the word contains each required letter in the required position
  return correctLetterData.every(data => word.toUpperCase()[data.position] === data.letter.toUpperCase())
}

function wordRespectsIncorrectLetterData(word) {
  // grab all store objects corresponding to required letters
  var incorrectLetterData = store.value.filter(el => el.flag === 1)
  // map to just {letter, position}; drop the flag
  incorrectLetterData = incorrectLetterData.map( el => {
    return {letter: el.letter, position: el.position}
  })

  // check whether the word contains EACH incorrect letter and NOT in the incorrect position
  // returns TRUE if and only FOR EACH incorrectly-positioned letter
  // (1) the word contains the incorrectly-positioned letter
  // AND
  // (2) doesn't contain the letter in the incorrect position
  return incorrectLetterData.every(data => (
    word.toUpperCase().includes(data.letter.toUpperCase()) && 
    word.toUpperCase()[data.position] !== data.letter.toUpperCase()
  ))
}

const filteredWords= computed(() => {
  // create a new array of words which pass the test
  // 
  return words.filter(word => (
    wordAvoidsAbsentLetters(word) &&
    wordCorrectlyContainsRequiredLetter(word) &&
    wordRespectsIncorrectLetterData(word)
  ))
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
  <div
    class="pt-2 md:pt-4"
  >
    Potential Solutions: {{ filteredWords.length }}
    <button
      class="bg-white border rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400 mt-4 uppercase w-16"
      @click="toggleKeyboard"
    >
      {{ buttonMessage }}
    </button>
  </div>
  <div
    v-if="filteredWords.length && filteredWords.length < 500 && !showKeyboard"
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
  <div
    v-if="filteredWords.length === 0 && !showKeyboard"
    class="pt-2 md:pt-4"
  >
    No potential solutions.
  </div>
</template>