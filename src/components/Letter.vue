<script setup>
import alphabet from '../resources/Alphabet';

  const props = defineProps({
    letter: {
      type: String,
      default: "",
      validator(value) {
        return value.length === 0 || (value.length === 1 && alphabet.includes(value.toUpperCase(0)))
      }
    },
    id: {
      type: String,
      default: `${Date.now()}`
    },
    flag: {
      type: String,
      default: "absent",
      validator(value) {
        return ["absent", "present", "correct"].includes(value)
      }
    }
  })
</script>

<template>
  <div class="m-1 bg-black h-[42px] w-[42px] flex flex-row justify-center">
    <div
      class="text-3xl"
    >
      {{ letter }}
    </div>
  </div>
</template>