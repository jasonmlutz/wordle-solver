<script setup>
import {reactive} from "vue"
import { v4 as uuid } from 'uuid';

import alphabet from "./resources/Alphabet"
import Letter from "./components/Letter.vue";

const letters = reactive(alphabet.slice(0,6).map(el => (
  {letter: el, id: uuid()})))
</script>

<template>
  <header class="text-xl text-center pt-4">
    Wordle Solver
  </header>
  <div class="flex flex-col items-center h-4/7 pt-4">
    <ul class="flex flex-row justify-between w-[300px]">
      <li
        v-for="letter in letters"
        :key="letter.id"
        class="w-[50px] h-[50px]"
      >
        <Letter
          :key="letter.id"
          :letter="letter.letter"
        />
      </li>
    </ul>
  </div>
</template>