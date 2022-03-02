<script setup>
import {ref, computed, provide} from "vue"
import { v4 as uuid } from 'uuid';

import ValidRows from "./components/ValidRows.vue";
import EmptyRows from "./components/EmptyRows.vue";
import Solution from "./components/Solution.vue";

const submittedRows = ref([]);

const emptyRowCount= computed(() => {
  return 6 - submittedRows.value.length
})

const newWord = ref("")

function handleSubmit() {
  if (newWord.value.length !== 5) {
    window.alert("please enter a 5-letter word")
  } else {
    console.log(newWord.value.toUpperCase())
    submittedRows.value.push(generateRow(newWord.value.toUpperCase()))
    newWord.value = ""
  }
}

function generateRow(word) {
  let letters = word.split("").map((letter, index) => (
    {letter: letter, id: uuid(), position: index}
  ))
  return {letters: letters, id: submittedRows.value.length + 1}
}

const store = ref([])

function updateStore(letter, position, flag) {
  store.value = store.value.filter(el => (
    !(el.position === position && el.letter === letter)
  ))
  store.value.push({letter, position, flag})
}

provide("store", {store, updateStore})


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
      <div id="letterGrid">
        <ValidRows
          v-if="submittedRows.length > 0"
          :rows="submittedRows"
        />
        <EmptyRows
          v-if="submittedRows.length < 6"
          :count="emptyRowCount"
        />
      </div>
      <div
        v-if="submittedRows.length < 6"
        id="inputContainer"
        class="pt-4 flex justify-center w-[300px]"
      >
        <input
          v-model="newWord"
          type="text"
          placeholder="5-letter guess"
          class="text-black px-1 uppercase flex-1"
          @keyup.enter="handleSubmit"
        >
        <button
          class="bg-white border rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400"
          @click="handleSubmit"
        >
          submit
        </button>
      </div>
      <Solution v-if="submittedRows.length > 0" />
    </div>
  </div>
</template>