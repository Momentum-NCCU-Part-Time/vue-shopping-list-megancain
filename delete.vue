<script setup>
import { ref, toRefs } from 'vue'

const props = defineProps({
  list: Object
})
const { list } = toRefs(props)
const emit = defineEmits(['listDeleted'])

const deleting = ref(false)

async function deleteList() {
  try {
    const response = await fetch(`http://localhost:3000/lists/${list.value.id}`, {
      method: 'DELETE'
    });

    if (!response.ok) {
      throw new Error('Network response was not ok');
    }

    const deletedList = await response.json();
    emit('listDeleted', deletedList);
  } catch (error) {
    console.error('There has been a problem with your fetch operation:', error);
  }
}

function toggleDeleting() {
  deleting.value = !deleting.value;
}
</script>

<template>
  <div>
    <button v-if="deleting" class="btn" @click="toggleDeleting">Keep List</button>
    <button v-else class="confirmDeleteBtn" @click="toggleDeleting">Delete List</button>
    <div v-if="deleting">
      <button @click="deleteList"> delete list</button>
    </div>
  </div>
</template>