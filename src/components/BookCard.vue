<template>
  <div :class="['book-card', { completed: book.completed, favorite: book.favorite }]">
    <div class="book-info">
      <h3>
        {{ book.title }}
        <span v-if="book.favorite" class="fav-star">★</span>
      </h3>
      <p class="book-author">{{ book.author }}</p>
      <span class="book-genre">{{ book.genre }}</span>
    </div>

    <div class="book-actions">
      <!-- Рейтинг (только если прочитана) -->
      <div v-if="book.completed" class="rating">
        <span
          v-for="star in 5"
          :key="star"
          @click="$emit('rate', star)"
          :title="`Оценить ${star}/5`"
        >{{ star <= book.rating ? '★' : '☆' }}</span>
      </div>

      <!-- Избранное -->
      <button
        :class="['btn', 'btn-fav', { 'is-fav': book.favorite }]"
        @click="$emit('favorite')"
        :title="book.favorite ? 'Убрать из избранного' : 'В избранное'"
      >{{ book.favorite ? '★' : '☆' }}</button>

      <!-- Прочитано/нет -->
      <button
        :class="['btn', book.completed ? 'btn-done' : 'btn-primary']"
        @click="$emit('toggle')"
      >{{ book.completed ? '✓ Прочитано' : 'Отметить' }}</button>

      <!-- Удалить -->
      <button class="btn btn-danger" @click="$emit('delete')">✕</button>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate', 'favorite'])
</script>

<style scoped>
.book-card {
  background: white;
  border-radius: 10px;
  padding: 16px;
  margin-bottom: 10px;
  box-shadow: 0 2px 6px rgba(0,0,0,0.07);
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.25s;
  border-left: 4px solid #ddd;
}
.book-card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.12);
}
.book-card.completed {
  border-left-color: #4CAF50;
  background: #f8fdf8;
}
.book-card.favorite {
  border-left-color: #FFD700;
}
.book-card.completed.favorite {
  border-left-color: #FFD700;
}

.book-info { flex: 1; }
.book-info h3 { font-size: 16px; color: #333; margin-bottom: 4px; }
.fav-star { color: #FFD700; font-size: 14px; margin-left: 4px; }
.book-author { color: #777; font-size: 13px; margin-bottom: 4px; }
.book-genre {
  display: inline-block;
  background: #eee;
  padding: 2px 8px;
  border-radius: 10px;
  font-size: 12px;
  color: #666;
}

.book-actions {
  display: flex;
  align-items: center;
  gap: 8px;
  flex-wrap: wrap;
  justify-content: flex-end;
}

.rating { display: flex; gap: 2px; }
.rating span {
  font-size: 20px;
  cursor: pointer;
  color: #FFD700;
  transition: transform 0.1s;
}
.rating span:hover { transform: scale(1.3); }

.btn {
  padding: 7px 12px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s;
  white-space: nowrap;
}
.btn-primary { background: #4CAF50; color: white; }
.btn-primary:hover { background: #45a049; }
.btn-done { background: #2196F3; color: white; }
.btn-done:hover { background: #1e87db; }
.btn-fav { background: #FFF8DC; color: #888; border: 1px solid #ddd; }
.btn-fav.is-fav { background: #FFD700; color: white; border-color: #FFD700; }
.btn-fav:hover { background: #FFE066; }
.btn-danger { background: #f44336; color: white; }
.btn-danger:hover { background: #da190b; }
</style>
