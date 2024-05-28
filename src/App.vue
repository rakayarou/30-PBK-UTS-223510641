<!-- App.vue -->
<script setup>
import { ref, onMounted, computed, watch, provide } from 'vue'
import TodosInput from './components/TodosInput.vue'
import Posts from './components/Posts.vue'

const todos = ref([])
provide('todos', todos)

const name = ref('')

const todos_asc = computed(() => todos.value.sort((a,b) =>{
  return a.createdAt - b.createdAt
}))

const activeMenu = ref('todos')

const showTodos = () => {
  activeMenu.value = 'todos'
}

const showPosts = () => {
  activeMenu.value = 'posts'
}

onMounted(() => {
  name.value = localStorage.getItem('name') || ''
  todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

watch(name, (newVal) => {
  localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
  localStorage.setItem('todos', JSON.stringify(newVal))
}, {
  deep: true
})
</script>

<template>
  <div id="app">
    <!-- Header -->
    <header>
      <nav>
        <ul>
          <li @click="showTodos">Todos</li>
          <li @click="showPosts">Posts</li>
        </ul>
      </nav>
    </header>

    <!-- Main Content -->
    <main>
      <div v-if="activeMenu === 'todos'">
        <TodosInput>
          <template #form-title>
            <h4>Custom List Aktivitas</h4>
          </template>
          <template #category-title>
            <h4>Custom Tugas</h4>
          </template>
          <template #submit-button>
            <input type="submit" value="Add Custom Todo" />
          </template>
        </TodosInput>
      </div>
      <div v-else-if="activeMenu === 'posts'">
        <Posts>
          <template #posts-title>
            <h2>Custom Postingan</h2>
          </template>
        </Posts>
      </div>
    </main>
  </div>

  <main class="app">
    <section class="greeting">
      <h2 class="title">TO DO LIST RAKA</h2>
    </section>

    <section class="todo-list">
      <h3>AKTIVITAS</h3>
      <div class="list" id="todo-list">
        <div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">
          <label>
            <input type="checkbox" v-model="todo.done" />
            <span :class="`bubble ${
              todo.category == 'Harian' 
                ? 'Harian' 
                : 'Kuliah'
            }`"></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todo.content" />
          </div>

          <div class="actions">
            <button class="edit" @click="editTodo(todo)">Edit</button>
            <button class="delete" @click="removeTodo(todo)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
/* Add your styles here */
</style>
