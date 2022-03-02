<script setup>
import {computed} from "vue"

const props = defineProps({
  count: {
    type: Number,
    default: 1,
    validator(value) {
      return Array.from(Array(31)).map((_, i) => i).includes(value)
    }
  }
})

const emptyLetters = computed(() => (
  Array.from(Array(props.count)).map((_, i) => {
    return {letter: " ", key: i}
  })
))

</script>

<template>
  <li
    v-for="letter in emptyLetters"
    :key="letter.id"
    class="w-[60px] h-[60px]"
  >
    <div class="m-1 h-[52px] w-[52px] flex flex-row justify-center bg-black border-2 border-gray-600">
      <div
        class="text-3xl pt-1.5 uppercase"
      >
        {{ letter.letter }}
      </div>
    </div>
  </li>
</template>