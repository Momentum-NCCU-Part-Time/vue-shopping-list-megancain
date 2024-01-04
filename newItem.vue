<script setup>
import { ref, toRefs } from 'vue'

const newItem = ref('')
const addingItem = ref(false)
const props = defineProps({ list: Object })
const emit = defineEmits(['itemAdded'])
const { list } = toRefs(props)

async function addNewItem() {
  const updatedList = {
    ...list.value,
    items: [
      ...list.value.items,
      {
        id: list.value.items.length + 1,
        itemName: newItem.value,
        purchased: false
      }
    ],
    updatedAt: new Date()
  };

  try {
    const response = await fetch(`http://localhost:3000/lists/${list.value.id}`, {
      method: 'PATCH',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(updatedList)
    });

    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    const addedItem = await response.json();
    emit('itemAdded', addedItem);
    newItem.value = '';
  } catch (error) {
    console.error('There was a problem with your fetch operation:', error);
  }
}

const toggleAddingItem = () => {
  addingItem.value = !addingItem.value;
}
</script>

<template>
  <div>
    <form v-if="addingItem" class="itemForm" @submit.prevent="addNewItem">
      <input v-model="newItem" type="text" placeholder="Add Item" />
      <button type="submit">Add</button>
    </form>
    <button @click="toggleAddingItem">{{ addingItem ? 'Cancel Item' : 'Add Item' }}</button>
  </div>
</template>
