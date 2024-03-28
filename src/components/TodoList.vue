<script lang="ts" setup>
import ListItem from "./ListItem.vue";
import { ref, computed, onMounted } from "vue";
import type { Ref } from "vue";

type Item = {
  title: string;
  checked?: boolean;
};

const storageItems: Ref<Item[]> = ref([]);

const getFromStorage = (): Item[] | [] => {
  const stored = localStorage.getItem("list-items");
  if (stored) {
    return JSON.parse(stored);
  }
  return [];
};

// Load items from localStorage and set storageItems.value
storageItems.value = getFromStorage();

const initialiseListItems = (): void => {
  // If there are no items in localStorage, initialize with default items
  if (storageItems.value?.length === 0) {
    const listItems = [
      { title: "Make a todo list app", checked: false },
      { title: "Predict the weather", checked: false },
      { title: "Listen to the Quran", checked: false },
      { title: "Let's get cooking", checked: false },
      { title: "Pump some iron", checked: false },
      { title: "Track my expenses", checked: false },
      { title: "Organize a half court", checked: false },
      { title: "Learn a new language", checked: false },
      { title: "Publish my work" },
    ];
    storageItems.value = listItems;
    setToStorage(listItems);
  }
};

// Update localStorage when an item is checked/unchecked
const updateItem = (item: Item): void => {
  const updatedItem = findItemInList(item);
  if (updatedItem) {
    toggleItemChecked(updatedItem);
    setToStorage(storageItems.value);
  }
};

const findItemInList = (item: Item): Item | undefined => {
  return storageItems.value.find(
    (itemInList: Item) => itemInList.title === item.title
  );
};

const toggleItemChecked = (item: Item): void => {
  item.checked = !item.checked;
};

// Compute sorted list based on checked status
const sortedList = computed(() =>
  [...storageItems.value].sort(
    (a, b) => (a.checked ? 1 : 0) - (b.checked ? 1 : 0)
  )
);

const setToStorage = (items: Item[]): void => {
  localStorage.setItem("list-items", JSON.stringify(items));
};

// Initialize list items on component mount
onMounted(() => {
  initialiseListItems();
});
</script>

<template>
  <ul>
    <li :key="key" v-for="(item, key) in sortedList">
      <ListItem
        :is-checked="item.checked"
        v-on:click.prevent="updateItem(item)"
        >{{ item.title }}</ListItem
      >
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
}
li {
  margin: 0.4rem 0;
}
</style>
