<script setup>
import { inject, computed } from "vue"

const {store} = inject("store")

// correct
const correctLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 2)
    .map(el => el.letter)
  return [...new Set(letters)].sort()
})

// incorrect
const incorrectLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 1)
    .map(el => el.letter)
    .filter(letter => (
      !correctLetters.value.includes(letter)
    ))
  return [...new Set(letters)].sort()
})

// absent
const absentLetters = computed(() => {
  const letters = store.value.filter(el => el.flag === 0)
    .map(el => el.letter)
    .filter(letter => (
      !correctLetters.value.includes(letter) &&
      !incorrectLetters.value.includes(letter)
    ))
  return [...new Set(letters)].sort()
})


</script>

<template>
  <ul
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
  </ul>
</template>