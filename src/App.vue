<script setup>
import {ref, computed} from "vue"
import { v4 as uuid } from 'uuid';

import ValidRows from "./components/ValidRows.vue";
import EmptyRows from "./components/EmptyRows.vue";

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

</script>


<template>
  <header class="text-xl text-center pt-4">
    Wordle Solver
  </header>
  <div
    class="flex flex-col items-center pt-4"
  >
    <div id="letterGrid">
      <ValidRows :rows="submittedRows" />
      <EmptyRows :count="emptyRowCount" />
    </div>
    <div
      v-if="submittedRows.length < 6"
      id="inputContainer"
      class="pt-4 flex justify-center w-4/6"
    >
      <input
        v-model="newWord"
        type="text"
        placeholder="5-letter guess"
        class="text-black px-1 uppercase flex-1"
        @keyup.enter="handleSubmit"
      >
      <button
        class="bg-white border-2 border-gray-600 rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400"
        @click="handleSubmit"
      >
        submit
      </button>
    </div>
  </div>
</template>