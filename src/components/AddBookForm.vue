<template>
  <div class="add-form">
    <h2>Добавить новую книгу</h2>
    <form @submit.prevent="handleSubmit">
      <div class="form-row">
        <input
          v-model="formData.title"
          type="text"
          placeholder="Название книги"
          required
        />
        <input
          v-model="formData.author"
          type="text"
          placeholder="Автор"
          required
        />
        <select v-model="formData.genre" required>
          <option value="">Жанр</option>
          <option>Роман</option>
          <option>Фантастика</option>
          <option>Детектив</option>
          <option>Научная</option>
          <option>Поэзия</option>
          <option>Биография</option>
        </select>
        <button type="submit" class="btn-add">+ Добавить</button>
      </div>
    </form>
  </div>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: ''
})

const handleSubmit = () => {
  emit('add-book', { ...formData })
  formData.title = ''
  formData.author = ''
  formData.genre = ''
}
</script>

<style scoped>
.add-form {
  background: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.08);
  margin-bottom: 20px;
}
.add-form h2 {
  color: #333;
  margin-bottom: 16px;
  font-size: 16px;
}
.form-row {
  display: flex;
  gap: 10px;
  flex-wrap: wrap;
}
.form-row input,
.form-row select {
  flex: 1;
  min-width: 140px;
  padding: 10px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
  outline: none;
  transition: border 0.2s;
}
.form-row input:focus,
.form-row select:focus {
  border-color: #667eea;
}
.btn-add {
  padding: 10px 20px;
  background: #667eea;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  white-space: nowrap;
  transition: background 0.2s;
}
.btn-add:hover {
  background: #5a6fd8;
}
</style>
