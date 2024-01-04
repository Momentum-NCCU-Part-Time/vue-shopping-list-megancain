    <script setup>
import { ref } from 'vue';
import Delete from './Delete.vue';

const props = defineProps({ list: Object });
const editing = ref(false);

const togglePurchased = (item) => {
  item.purchased = !item.purchased;
  updateList();
};

const toggleEditing = (value) => {
  editing.value = value;
};

const updateList = () => {
  fetch(`http://localhost:3000/lists/${props.list.id}`, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      title: props.list.title,
      items: [...props.list.items],
      updatedAt: new Date(),
    }),
  })
    .then((res) => res.json())
    .then(() => {});
};
</script>

<template>
  <div>
    <h3>{{ props.list.title }}: {{ props.list.items.length }} Items</h3>
    <div>
      <button v-if="editing" @click="toggleEditing(false)">Hide List</button>
      <button v-else @click="toggleEditing(true)">Show List</button>
      <div v-if="editing">
        <ul>
          <li
            v-for="item in props.list.items"
            :key="item.id"
            @click="togglePurchased(item)"
            :class="{ strikeout: item.purchased }"
          >
            {{ item.itemName }}
            <input v-model="item.purchased" type="checkbox" />
          </li>
        </ul>
        <!-- <Delete :list="props.list" /> -->
      </div>
    </div>
  </div>
</template>

<style>
.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}
</style>