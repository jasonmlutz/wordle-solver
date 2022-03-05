<script setup>
import { inject, computed, ref, watch } from "vue"
import {words} from "../resources/Words"

const props = defineProps({
  toggleKeyboard: {
    type: Function,
    default: null
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

const { settings } = inject("settings")

function filterWords() {
  return words.filter(word => (
    wordAvoidsAbsentLetters(word) &&
    wordCorrectlyContainsRequiredLetter(word) &&
    wordRespectsIncorrectLetterData(word)
  ))
}

const filteredWords = ref(words)
const refreshRequired = ref(true);

watch(
  store,
  () => {
    if (settings.value.autoReload) {
      filteredWords.value = filterWords()
    } else {
      refreshRequired.value = true
    }
  },
  {deep: true}
)

const buttonMessage = computed(() => {
  return settings.value.autoReload ? (
    props.showKeyboard ? "show" : "hide"
  ) : (
    props.showKeyboard ? (
      refreshRequired.value ? ("calculate potential solutions") : ("view potential solutions")
    ) : "show keyboard"
  )
})

const showLoading = ref(false)

function updateFilteredWords() {
  showLoading.value = true
  const newFilteredWords = filterWords()
  filteredWords.value = newFilteredWords
  refreshRequired.value = false
  showLoading.value = false
}


</script>

<template>
  <div
    v-if="settings.autoReload"
    class="flex flex-col items-center"
  >
    <div
      class="pt-2 md:pt-4"
    >
      Potential Solutions: {{ filteredWords.length }}
      <button
        class="bg-white border rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400 uppercase w-16"
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
      class="pt-2 md:pt-4 mx-4"
    >
      Solutions will be displayed when there are fewer than 500 options.
    </div>
    <div
      v-if="filteredWords.length === 0 && !showKeyboard"
      class="pt-2 md:pt-4"
    >
      No potential solutions.
    </div>
  </div>
  <div
    v-else
    class="flex flex-col items-center"
  >
    <div class="pt-2 md:pt-4">
      <button
        class="bg-white border rounded-md text-black px-1 uppercase hover:bg-gray-400 uppercase"
        @click="toggleKeyboard"
      >
        {{ buttonMessage }}
      </button>
    </div>
    <div
      v-if="!showKeyboard"
    >
      <div
        v-if="
          refreshRequired"
        class="pt-2 md:pt-4"
      >
        <button
          class="bg-white border rounded-md text-black px-1 uppercase hover:bg-gray-400 uppercase"
          @click="updateFilteredWords"
        >
          Update List
        </button>
        <div
          v-if="showLoading"
          class="pt-2 md:pt-4 mx-4"
        >
          <svg
            class="animate-spin m-[5px] h-16 w-16 mx-auto"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            />
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            />
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            />
          </svg>
        </div>
      </div>
      <div
        v-else
        class="pt-2 md:pt-4"
      >
        <div
          v-if="filteredWords.length && filteredWords.length < 500"
          class="pt-2 px-3 mt-2 uppercase bg-gray-800 h-[150px] w-[350px] overflow-y-auto"
        >
          {{ filteredWords.join(", ") }}
        </div>
        <div
          v-if="filteredWords.length >= 500"
          class="pt-2 md:pt-4 mx-4"
        >
          Solutions will be displayed when there are fewer than 500 options.
        </div>
        <div
          v-if="filteredWords.length === 0"
          class="pt-2 md:pt-4"
        >
          No potential solutions.
        </div>
      </div>
    </div>
  </div>
</template>