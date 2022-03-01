<script setup>
function handleSubmit () {
  console.log("start search!")
}
</script>

<template>
  <div
    class="pt-4"
  >
    <button
      class="bg-white border-2 border-gray-600 rounded-md text-black px-1 uppercase ml-2 hover:bg-gray-400"
      @click="handleSubmit"
    >
      Find solution
    </button>
  </div>
</template>