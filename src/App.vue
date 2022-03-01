<script setup>
import {reactive, ref} from "vue"
import { v4 as uuid } from 'uuid';

import alphabet from "./resources/Alphabet"
import Letter from "./components/Letter.vue";
import EmptyRows from "./components/EmptyRows.vue";

const letters = reactive(alphabet.slice(0,5).map(el => (
  {letter: el, id: uuid()})))

const emptyRowCount = ref(5)
</script>


<template>
  <header class="text-xl text-center pt-4">
    Wordle Solver
  </header>
  <div
    class="flex flex-col items-center pt-4"
  >
    <div id="letterGrid">
      <ul class="flex flex-row justify-between w-[300px]">
        <li
          v-for="letter in letters"
          :key="letter.id"
          class="w-[60px] h-[60px]"
        >
          <Letter
            :key="letter.id"
            :letter="letter.letter"
          />
        </li>
      </ul>
      <EmptyRows :count="emptyRowCount" />
    </div>
    <div
      id="inputContainer"
      class="pt-4 flex justify-center w-4/6"
    >
      <input
        type="text"
        placeholder="5-letter guess"
        class="text-black px-1 uppercase flex-1"
      >
      <button class="bg-white border-2 border-gray-600 rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400">
        submit
      </button>
    </div>
  </div>
</template>