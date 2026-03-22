<template>
  <div class="app">
    <header>
      <h1>📚 Менеджер книг</h1>
      <p>Управляй своей библиотекой</p>
    </header>

    <AddBookForm @add-book="addBook" />

    <BookFilters
      v-model:searchQuery="searchQuery"
      v-model:filter="currentFilter"
      :books="books"
    />

    <div v-if="filteredBooks.length === 0" class="empty">
      <div class="emoji">📖</div>
      <p>{{ books.length === 0 ? 'Добавьте первую книгу!' : 'Ничего не найдено' }}</p>
    </div>

    <div v-else>
      <BookCard
        v-for="book in filteredBooks"
        :key="book.id"
        :book="book"
        @toggle="toggleBook(book.id)"
        @delete="deleteBook(book.id)"
        @rate="rateBook(book.id, $event)"
        @favorite="favoriteBook(book.id)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const currentFilter = ref('all')
const searchQuery = ref('')

// Загрузка из localStorage
onMounted(() => {
  try {
    const saved = localStorage.getItem('books-p15')
    if (saved) books.value = JSON.parse(saved)
  } catch { /* ignore */ }
})

// Сохранение при изменении
watch(books, (val) => {
  localStorage.setItem('books-p15', JSON.stringify(val))
}, { deep: true })

const addBook = (bookData) => {
  if (!bookData.title.trim() || !bookData.author.trim()) {
    alert('Заполните название и автора')
    return
  }
  books.value.push({
    id: Date.now(),
    title: bookData.title.trim(),
    author: bookData.author.trim(),
    genre: bookData.genre || 'Другое',
    completed: false,
    favorite: false,
    rating: 0,
  })
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) {
    book.completed = !book.completed
    if (!book.completed) book.rating = 0
  }
}

const favoriteBook = (id) => {
  const book = books.value.find(b => b.id === id)
  if (book) book.favorite = !book.favorite
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)
  if (book && book.completed) book.rating = rating
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?'))
    books.value = books.value.filter(b => b.id !== id)
}

const filteredBooks = computed(() => {
  return books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed
      if (currentFilter.value === 'read') return book.completed
      if (currentFilter.value === 'favorite') return book.favorite
      return true
    })
    .filter(book => {
      if (!searchQuery.value.trim()) return true
      const q = searchQuery.value.toLowerCase()
      return book.title.toLowerCase().includes(q) || book.author.toLowerCase().includes(q)
    })
})
</script>

<style>
* { margin: 0; padding: 0; box-sizing: border-box; }
body { font-family: 'Segoe UI', Arial, sans-serif; background: #f0f2f5; min-height: 100vh; }

.app { max-width: 820px; margin: 0 auto; padding: 20px; }

header {
  text-align: center;
  padding: 24px 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 12px;
  margin-bottom: 24px;
  box-shadow: 0 4px 12px rgba(102,126,234,0.4);
}
header h1 { font-size: 2em; margin-bottom: 4px; }
header p { opacity: 0.85; font-size: 14px; }

.empty { text-align: center; padding: 50px 20px; color: #bbb; }
.empty .emoji { font-size: 52px; margin-bottom: 12px; }
</style>
