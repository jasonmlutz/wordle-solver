<script setup>
import {ref} from "vue"

const name = ref("");
const displayName = ref("");

const nameInput = ref(null);

function handleSubmit() {
  displayName.value = name.value;
  nameInput.value.blur()
}

</script>

<template> 
  <div>
    <input
      ref="nameInput"
      v-model="name"
      class="text-black mx-2 mb-2 p-2 border border-black rounded-md"
      placeholder="enter a name"
      @keyup.enter="handleSubmit"
    ><button
      class="bg-white text-black mx-2 mb-2 p-2 border border-black rounded-md hover:bg-gray-200"
      @click="handleSubmit"
    >
      SUBMIT
    </button>
    <p class="p-2 mb-2">
      Name: {{ displayName }}
    </p>
  </div>
</template>

