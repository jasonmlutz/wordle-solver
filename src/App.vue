<script setup>
import {ref, computed, provide, onMounted } from "vue"
import { v4 as uuid } from 'uuid';

import alphabet from "./resources/Alphabet";

import ValidSquares from "./components/ValidSquares.vue";
import EmptySquares from "./components/EmptySquares.vue";
import Solution from "./components/Solution.vue";
import Keyboard from "./components/Keyboard.vue"

onMounted(() => {
    window.addEventListener("keypress", function(e) {
      const letter = String.fromCharCode(e.keyCode);
      if (alphabet.includes(letter.toUpperCase())) {
        handleSubmit(letter)
      }
    }.bind(this));
    window.addEventListener("keydown", function(e) {
      var key = e.keyCode || e.charCode;
      if ( key === 8 || key === 46) {
        deleteLastLetter()
      }
    })
  }
)

const submittedLetters = ref([]);

const showKeyboard = ref(true);
function toggleKeyboard() {
  showKeyboard.value = !showKeyboard.value
}

const emptySquareCount= computed(() => {
  return 30 - submittedLetters.value.length - 1
})

function handleSubmit(letter) {
  if (submittedLetters.value.length < 30 && alphabet.includes(letter.toUpperCase())) {
    const newLetter = {
      letter: letter, 
      id: uuid(), 
      position: (submittedLetters.value.length) % 5, 
      index: submittedLetters.value.length,
    }
    submittedLetters.value.push(newLetter)
  }
}

function deleteLastLetter() {
  const lastLetter = submittedLetters.value.pop()
  store.value = store.value.filter(data => data.index !== lastLetter.index)
}

const store = ref([])
// store will be an array of the form 
// store[index] = {letter, position, flag}

function updateStore(letter, position, flag, index) {
  store.value[index] = {letter, position, flag}
}

provide("store", {store, updateStore})

  function clearAllLetters() {
    if (window.confirm("Do you really want to clear all letters?")) {
      submittedLetters.value = []
      store.value = []
    }
  }

</script>


<template>
  <div
    id="app"
    class="mx-auto bg-black h-screen"
  >
    <header class="text-xl text-center pt-2 md:pt-4">
      Wordle Solver
    </header>
    <div
      class="flex flex-col items-center pt-2 md:pt-4"
    >
      <ul
        id="letterGrid"
        class="flex flex-row justify-between w-[300px] flex-wrap"
      >
        <ValidSquares
          :letters="submittedLetters"
        />
        <li
          v-if="emptySquareCount >= 0"
          id="cursorSquare"
          class="w-[60px] h-[60px]"
        >
          <div class="m-1 h-[52px] w-[52px] flex flex-row justify-center bg-black border-2 border-gray-200 animate-pulse">
            <div
              class="text-3xl pt-1.5 uppercase"
            >
              {{ " " }}
            </div>
          </div>
        </li>
        <EmptySquares
          v-if="emptySquareCount >= 0"
          :count="emptySquareCount"
        />
      </ul>
      <Solution
        :show-keyboard="showKeyboard"
        :toggle-keyboard="toggleKeyboard"
      />
      <Keyboard
        v-if="showKeyboard"
        :delete-last-letter="deleteLastLetter"
        :handle-submit="handleSubmit"
        :toggle-keyboard="toggleKeyboard"
        :clear-all-letters="clearAllLetters"
      />
    </div>
  </div>
</template>