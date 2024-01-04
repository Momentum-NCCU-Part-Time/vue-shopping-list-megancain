    <script setup>
import { ref } from 'vue';
import NewListItems from './NewListItems.vue';

const newList = ref('');
const addingList = ref(false);
const emit = defineEmits(['listAdded']);

async function addNewList() {
  try {
    const response = await fetch('http://localhost:3000/lists/', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ title: newList.value, items: [], updatedAt: new Date() }),
    });

    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    const createdList = await response.json();
    emit('listAdded', createdList);
    resetList();
  } catch (error) {
    console.error('There was a problem with your fetch operation:', error);
  }
}

const toggleAddingList = () => {
  addingList.value = !addingList.value;
};

const resetList = () => {
  newList.value = '';
};
</script>

<template>
  <div>
    <button @click="toggleAddingList">{{ addingList ? 'Nevermind' : 'New List' }}</button>
    <form v-if="addingList" class="newList" @submit.prevent="addNewList">
      <input v-model="newList" type="text" placeholder="New List Title" />
      <!-- <div>
        <NewListItems />
        <NewListItems />
        <NewListItems />
        <NewListItems />
        <NewListItems />
      </div> -->
      <button type="submit">Add</button>
    </form>
  </div>
</template>