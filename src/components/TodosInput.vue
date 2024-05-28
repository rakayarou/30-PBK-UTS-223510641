<!-- TodosInput.vue -->
<template>
  <section class="create-todo">
    <form id="new-todo-form" @submit.prevent="addTodo">
      <slot name="form-title">
        <h4>LIST AKTIVITAS</h4>
      </slot>
      <input 
        type="text" 
        name="content" 
        id="content" 
        placeholder="Ketik Disini"
        v-model="input_content" 
      />
      <slot name="category-title">
        <h4>TUGAS</h4>
      </slot>
      <div class="options">
        <label>
          <input 
            type="radio" 
            name="category" 
            id="category1" 
            value="Harian"
            v-model="input_category" 
          />
          <span class="bubble Harian"></span>
          <div>Harian</div>
        </label>
        <label>
          <input 
            type="radio" 
            name="category" 
            id="category2" 
            value="Kuliah"
            v-model="input_category" 
          />
          <span class="bubble Kuliah"></span>
          <div>Kuliah</div>
        </label>
      </div>
      <slot name="submit-button">
        <input type="submit" value="Add Todo" />
      </slot>
    </form>
  </section>
</template>

<script setup>
import { ref, inject } from 'vue'

const todos = inject('todos')
const input_content = ref('')
const input_category = ref(null)

const addTodo = () => {
  if (input_content.value.trim() === '' || input_category.value === null) {
    return
  }

  todos.value.push({
    content: input_content.value,
    category: input_category.value,
    done: false,
    editable: false,
    createdAt: new Date().getTime()
  })

  input_content.value = ''
  input_category.value = null
}
</script>

<style scoped>
/* Add your styles here */
</style>
