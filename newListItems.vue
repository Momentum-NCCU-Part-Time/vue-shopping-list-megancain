    <script setup>
import { ref } from 'vue';

const newListItem = ref('');
</script>

<template>
  <input 
    v-model="newListItem" 
    type="text" 
    placeholder="New Item" 
    aria-label="New List Item"
  />
</template>