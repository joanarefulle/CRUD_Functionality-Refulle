<script setup>
import { ref, onMounted } from 'vue'
import BookItem from './components/BookItem.vue'

const books = ref([])

const newTitle = ref('')
const newAuthor = ref('')
const newCategory = ref('')
const newYear = ref('')

function addBook() {
  if (
    newTitle.value.trim() === '' ||
    newAuthor.value.trim() === '' ||
    newCategory.value.trim() === '' ||
    newYear.value === ''
  ) {
    return
  }

  books.value.push({
    id: Date.now(),
    title: newTitle.value,
    author: newAuthor.value,
    category: newCategory.value,
    year: newYear.value,
    available: true
  })

  newTitle.value = ''
  newAuthor.value = ''
  newCategory.value = ''
  newYear.value = ''

  saveBooks()
}

function deleteBook(id) {
  books.value = books.value.filter(
    book => book.id !== id
  )

  saveBooks()
}

function updateBook(updatedBook) {
  const index = books.value.findIndex(
    book => book.id === updatedBook.id
  )

  if (index !== -1) {
    books.value[index] = updatedBook
  }

  saveBooks()
}

function saveBooks() {
  localStorage.setItem(
    'books',
    JSON.stringify(books.value)
  )
}

onMounted(() => {
  const savedBooks = localStorage.getItem('books')

  if (savedBooks) {
    books.value = JSON.parse(savedBooks)
  }
})
</script>


<template>

  <div class="container">

    <h1>📚 My Library</h1>

    <!-- Add Book -->

    <div class="add-book">

      <input
        v-model="newTitle"
        type="text"
        placeholder="Book title"
      />

      <input
        v-model="newAuthor"
        type="text"
        placeholder="Author"
      />

      <input
        v-model="newCategory"
        type="text"
        placeholder="Category"
      />

      <input
        v-model="newYear"
        type="number"
        placeholder="Year"
      />

      <button @click="addBook">
        Add Book
      </button>

    </div>


    <!-- Book List -->

    <div class="book-list">

      <BookItem
        v-for="book in books"
        :key="book.id"
        :book="book"
        @delete="deleteBook"
        @update="updateBook"
      />

    </div>

  </div>

</template>


<style>

.container {
  width: 600px;

  margin: 50px auto;

  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;

  color: #4c1d95;
}

.add-book {
  display: flex;

  flex-direction: column;

  gap: 10px;

  margin-bottom: 20px;

  padding: 20px;

  background: #f5f3ff;

  border-radius: 8px;
}

.add-book input {
  padding: 10px;

  border: 1px solid #ccc;

  border-radius: 5px;
}

.add-book button {
  padding: 10px 15px;

  background: #7c3aed;

  color: white;

  border: none;

  border-radius: 5px;

  cursor: pointer;
}

.add-book button:hover {
  background: #6d28d9;
}

.book-list {
  display: flex;

  flex-direction: column;

  gap: 10px;
}

</style>