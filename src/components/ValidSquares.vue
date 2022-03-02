<script setup>
import Letter from "./Letter.vue"
import alphabet from "../resources/Alphabet";
import { v4 as uuid } from 'uuid';

// TODO prop validation; custom Letter object?
//  https://vuejs.org/guide/components/props.html#prop-validation
const props = defineProps({
  letters: {
    type: Array,
    default: undefined
  }
})

</script>

<template>
  <li
    v-for="letter in letters"
    :key="letter.id"
    class="w-[60px] h-[60px]"
  >
    <Letter
      :key="letter.id"
      :letter="letter.letter"
      :position="letter.position"
      :index="letter.index"
    />
  </li>
</template>