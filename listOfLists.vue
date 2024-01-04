    <script setup>
import { ref, onMounted } from 'vue';
import ListItems from './ListItems.vue';
import NewList from './NewList.vue';
import NewItem from './NewItem.vue';
import Delete from './Delete.vue';

const lists = ref([]);

const fetchLists = async () => {
  try {
    const response = await fetch('http://localhost:3000/lists/', {
      method: 'GET',
      headers: { 'Content-Type': 'application/json' }
    });
    if (!response.ok) {
      throw new Error('Failed to fetch lists');
    }
    lists.value = await response.json();
  } catch (error) {
    console.error('Error fetching lists:', error);
  }
};

onMounted(fetchLists);
</script>

<template>
  <div>
    <NewList @listAdded="fetchLists" />
    <div class="listPad">
      <div class="lists" v-for="list in lists" :key="list.id">
        <ListItems :list="list" @listDeleted="fetchLists" />
        <Delete :list="list" @listDeleted="fetchLists" />
        <NewItem :list="list" @itemAdded="fetchLists" />
      </div>
    </div>
  </div>
</template>