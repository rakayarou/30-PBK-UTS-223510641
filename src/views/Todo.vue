<template>
    <div id="app">
  
      <!-- Main Content -->
      <main>
        <div class="container">
          <h2 class="title">To Do List</h2>
          <!-- Form untuk menambahkan To Do -->
          <section class="create-todo">
            <form id="new-todo-form" @submit.prevent="addTodo">
              <h4>List Aktivitas</h4>
              <input type="text" name="content" id="content" placeholder="Ketik Disini" v-model="input_content" />
              <h4>Tugas</h4>
              <div class="options">
                <label>
                  <input type="radio" name="category" value="Harian" v-model="input_category" />
                  <span class="bubble Harian"></span>
                  <div>Harian</div>
                </label>
                <label>
                  <input type="radio" name="category" value="Kuliah" v-model="input_category" />
                  <span class="bubble Kuliah"></span>
                  <div>Kuliah</div>
                </label>
              </div>
              <input type="submit" value="Add Todo" />
            </form>
          </section>
          
          <!-- Daftar To Do -->
          <section class="todo-list">
            <h3>Aktivitas</h3>
            <div class="list">
              <div v-for="todo in todos_asc" :key="todo.createdAt" :class="{ 'todo-item': true, 'done': todo.done }">
                <label>
                  <input type="checkbox" v-model="todo.done" />
                  <span :class="['bubble', todo.category]"></span>
                </label>
                <div class="todo-content">
                  <input type="text" v-model="todo.content" />
                </div>
                <div class="actions">
                  <button class="delete" @click="removeTodo(todo)">Delete</button>
                </div>
              </div>
            </div>
          </section>
        </div>
      </main>
    </div>
  </template>
  
  <script setup>
  import { ref, computed, watch } from 'vue';
  
  const todos = ref([]);
  const input_content = ref('');
  const input_category = ref(null);
  const todos_asc = computed(() => todos.value.sort((a, b) => a.createdAt - b.createdAt));
  
  const addTodo = () => {
    if (input_content.value.trim() === '' || input_category.value === null) {
      return;
    }
  
    todos.value.push({
      content: input_content.value,
      category: input_category.value,
      done: false,
      createdAt: new Date().getTime(),
    });
  };
  
  const removeTodo = todo => {
    todos.value = todos.value.filter(t => t !== todo);
  };
  
  // Simulasi data To Do awal dari localStorage
  const initialTodos = JSON.parse(localStorage.getItem('todos')) || [];
  todos.value = initialTodos;
  
  // Watcher untuk menyimpan perubahan di localStorage
  watch(todos, newVal => {
    localStorage.setItem('todos', JSON.stringify(newVal));
  }, { deep: true });
  
  </script>
  
  <style scoped>
  #app {
    font-family: 'Arial', sans-serif;
  }
  
  main {
    display: flex;
    justify-content: center;
  }
  
  .container {
    max-width: 600px;
    margin: 20px;
  }
  
  h2 {
    margin-bottom: 10px;
  }
  
  h3 {
    margin-top: 10px;
  }
  
  .create-todo {
    background-color: #f8f9fa;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
  }
  
  .options {
    display: flex;
    margin-top: 10px;
  }
  
  .options label {
    display: flex;
    align-items: center;
    margin-right: 10px;
  }
  
  .options input[type="radio"] {
    display: none;
  }
  
  .options .bubble {
    display: inline-block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    margin-right: 5px;
  }
  
  .options .Harian {
    background-color: #007bff;
  }
  
  .options .Kuliah {
    background-color: #28a745;
  }
  
  .todo-list {
    background-color: #f8f9fa;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .list {
    margin-top: 10px;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    background-color: #ffffff;
    border: 1px solid #dee2e6;
    border-radius: 5px;
    padding: 10px;
    margin-bottom: 10px;
  }
  
  .todo-item.done {
    opacity: 0.6;
  }
  
  .todo-item label {
    margin-right: 10px;
  }
  
  .todo-item .bubble {
    margin-right: 10px;
  }
  
  .actions .delete {
    background-color: #dc3545;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 5px;
    cursor: pointer;
  }
  </style>
  