<template>
  <div class="filters">
    <input
      class="search-input"
      v-model="searchQuery"
      type="text"
      placeholder="🔍 Поиск по названию или автору..."
    />
    <div class="filter-btns">
      <button
        v-for="opt in filterOptions"
        :key="opt.value"
        :class="['filter-btn', { active: filter === opt.value }]"
        @click="$emit('update:filter', opt.value)"
      >{{ opt.label }}</button>
    </div>
    <div class="stats">
      Всего: {{ total }} | Прочитано: {{ completed }} | Осталось: {{ total - completed }}
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'books'])
defineEmits(['update:filter'])

const searchQuery = defineModel('searchQuery')

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' },
  { value: 'favorite', label: '★ Избранные' },
]

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(b => b.completed).length)
</script>

<style scoped>
.filters {
  background: white;
  padding: 14px 16px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  margin-bottom: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}
.search-input {
  flex: 1;
  min-width: 200px;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 20px;
  font-size: 14px;
  outline: none;
  transition: border 0.2s;
}
.search-input:focus {
  border-color: #667eea;
}
.filter-btns {
  display: flex;
  gap: 6px;
}
.filter-btn {
  padding: 6px 14px;
  border: 1px solid #ddd;
  background: white;
  border-radius: 20px;
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s;
}
.filter-btn:hover {
  background: #f0f0f0;
}
.filter-btn.active {
  background: #667eea;
  color: white;
  border-color: #667eea;
}
.stats {
  font-size: 13px;
  color: #888;
}
</style>
