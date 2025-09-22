<script setup lang="ts">
import type { ListItem } from './App.vue'

const props = defineProps<{ item: ListItem }>()

const toggleCompleted = () => {
  const item = props.item
  item.completedAt = item.completedAt ? null : new Date()
}
</script>

<template>
  <li style="display: flex; gap: 1em; align-items: center; flex-direction: row">
    <input
      type="checkbox"
      :id="item.id + '_checkbox'"
      :checked="item.completedAt !== null"
      @input="toggleCompleted()"
    />
    <span>{{ item.title }}</span>
  </li>
</template>
