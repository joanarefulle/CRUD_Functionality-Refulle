<script setup>
import { ref } from 'vue'

const props = defineProps({
  book: Object
})

const emit = defineEmits([
  'delete',
  'update'
])

const isEditing = ref(false)

const editedTitle = ref(props.book.title)
const editedAuthor = ref(props.book.author)
const editedCategory = ref(props.book.category)
const editedYear = ref(props.book.year)

function updateBook() {
  if (
    editedTitle.value.trim() === '' ||
    editedAuthor.value.trim() === '' ||
    editedCategory.value.trim() === '' ||
    editedYear.value === ''
  ) {
    return
  }

  emit('update', {
    ...props.book,
    title: editedTitle.value,
    author: editedAuthor.value,
    category: editedCategory.value,
    year: editedYear.value
  })

  isEditing.value = false
}

function toggleAvailable() {
  emit('update', {
    ...props.book,
    available: !props.book.available
  })
}

function deleteBook() {
  emit('delete', props.book.id)
}

function startEditing() {
  editedTitle.value = props.book.title
  editedAuthor.value = props.book.author
  editedCategory.value = props.book.category
  editedYear.value = props.book.year

  isEditing.value = true
}
</script>


<template>

  <div class="book-item">

    <!-- Normal Mode -->

    <div v-if="!isEditing">

      <h3>
        {{ book.title }}
      </h3>

      <p>
        <strong>Author:</strong>
        {{ book.author }}
      </p>

      <p>
        <strong>Category:</strong>
        {{ book.category }}
      </p>

      <p>
        <strong>Year:</strong>
        {{ book.year }}
      </p>

      <p>
        <strong>Status:</strong>

        <span
          :class="{
            available: book.available,
            borrowed: !book.available
          }"
        >
          {{ book.available ? 'Available' : 'Borrowed' }}
        </span>
      </p>

      <button @click="toggleAvailable">
        {{ book.available ? 'Mark as Borrowed' : 'Mark as Available' }}
      </button>

      <button @click="startEditing">
        Edit
      </button>

      <button @click="deleteBook">
        Delete
      </button>

    </div>


    <!-- Edit Mode -->

    <div v-else>

      <h3>Edit Book</h3>

      <input
        v-model="editedTitle"
        type="text"
        placeholder="Book title"
        @keyup.enter="updateBook"
      />

      <input
        v-model="editedAuthor"
        type="text"
        placeholder="Author"
      />

      <input
        v-model="editedCategory"
        type="text"
        placeholder="Category"
      />

      <input
        v-model="editedYear"
        type="number"
        placeholder="Publication year"
      />

      <button @click="updateBook">
        Save
      </button>

      <button @click="isEditing = false">
        Cancel
      </button>

    </div>

  </div>

</template>


<style scoped>

.book-item {
  padding: 15px;

  border: 1px solid #ddd;

  border-radius: 8px;

  background: white;
}

.book-item h3 {
  margin-top: 0;

  color: #4c1d95;
}

.book-item p {
  margin: 8px 0;
}

.book-item button {
  margin-right: 5px;

  margin-top: 8px;

  padding: 8px 12px;

  cursor: pointer;
}

.book-item input {
  display: block;

  width: 100%;

  box-sizing: border-box;

  margin-bottom: 10px;

  padding: 9px;
}

.available {
  color: green;

  font-weight: bold;
}

.borrowed {
  color: red;

  font-weight: bold;
}

</style>
