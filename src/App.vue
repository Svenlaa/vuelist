<script setup lang="ts">
import { ref, watch } from 'vue'
import TodoItem from './TodoItem.vue'

export type ListItem = {
  id: string
  title: string
  completedAt: Date | null
  createdAt: Date
}

const getItems = () => JSON.parse(localStorage.getItem('todo-items') ?? '[]') as ListItem[]
const setItems = (items: ListItem[]) =>
  localStorage.setItem('todo-items', JSON.stringify(items ?? []))

const items = ref<ListItem[]>(getItems())
watch(
  items,
  (newItems) => {
    setItems(newItems)
  },
  { deep: true },
)

const createListItem = () => {
  const item = {
    id: crypto.randomUUID(),
    title: title.value,
    completedAt: null,
    createdAt: new Date(),
  }
  // save to a hypothethical database;
  items.value.push(item)
  title.value = ''
}
const title = defineModel({ default: '', required: true })
</script>

<template>
  <h1>Vuelist</h1>
  <form @submit.prevent="createListItem()">
    <label>
      Title:
      <input v-model="title" required />
    </label>
    <button type="submit">Create and save</button>
  </form>
  <ul>
    <TodoItem v-for="item in items" :key="item.id" :item="item" />
  </ul>
  <p>
    {{ items.filter((item) => item.completedAt).length }} out of {{ items.length }} items have been
    completed
  </p>
</template>

<style scoped></style>
