<script lang="ts" setup>
import ListItem from "./ListItem.vue";

type Item = {
  title: string;
  checked?: boolean;
};

const listItems: Item[] = [
  { title: "Make a todo list app", checked: true },
  { title: "Predict the weather", checked: false },
  { title: "Listen to the Quran", checked: false },
  { title: "Let's get cooking", checked: false },
  { title: "Pump some iron", checked: false },
  { title: "Track my expenses", checked: false },
  { title: "Organize a half court", checked: false },
  { title: "Learn a new language", checked: false },
  { title: "Publish my work" },
];
</script>

<template>
  <ul>
    <li :key="key" v-for="(item, key) in listItems">
      <ListItem :is-checked="false">{{ item.title }}</ListItem>
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
