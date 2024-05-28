<script setup>
import { ref, onMounted, computed, watch } from 'vue'



const todos = ref([])
const name = ref('')

const input_content = ref('')
const input_category = ref(null)

const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

const activeMenu = ref('todos');
const users = ref([]);
const posts = ref([]);
const selectedUser = ref(null);

const showTodos = () => {
  activeMenu.value = 'todos';
};

const showPosts = () => {
  activeMenu.value = 'posts';
};

onMounted(() => {
  // Ambil data user dari API
  fetch('https://jsonplaceholder.typicode.com/users')
    .then(response => response.json())
    .then(data => {
      users.value = data;
    });

  // Ambil data postingan dari API
  fetch('https://jsonplaceholder.typicode.com/posts')
    .then(response => response.json())
    .then(data => {
      posts.value = data;
    });
});

const filteredPosts = computed(() => {
  // Filter postingan berdasarkan user yang dipilih
  return posts.value.filter(post => post.userId === parseInt(selectedUser.value));
});

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

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
}

const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}

const editTodo = (todo) => {
	todo.editable = true;
}

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
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
        <!-- Fitur Todos -->
        <h2>Todos</h2>
        <!-- Placeholder untuk implementasi fitur Todos -->
      </div>
      <div v-else-if="activeMenu === 'posts'">
        <!-- Fitur Postingan -->
        <h2>Postingan</h2>
        <select v-model="selectedUser">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
        <div v-if="selectedUser">
          <div v-for="post in filteredPosts" :key="post.id">
            <h3>{{ post.title }}</h3>
            <p>{{ post.body }}</p>
          </div>
        </div>
        <div v-else>
          <p>Silakan pilih pengguna untuk melihat postingan mereka.</p>
        </div>
      </div>
    </main>
  </div>

  <div id="app">
    <Parent />
  </div>


	<main class="app">
		
		<section class="greeting">
			<h2 class="title">
				TO DO LIST RAKA
			</h2>
		</section>

		<section class="create-todo">

			<form id="new-todo-form" @submit.prevent="addTodo">
				<h4>LIST AKTIVITAS</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="Ketik Disini"
					v-model="input_content" />
				
				<h4>TUGAS</h4>
				<div class="options">

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category1" 
							value="Harian"
							v-model="input_category" />
						<span class="bubble Harian"></span>
						<div>Harian</div>
					</label>

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category2" 
							value="Kuliah"
							v-model="input_category" />
						<span class="bubble Kuliah"></span>
						<div>Kuliah</div>
					</label>

				</div>

				<input type="submit" value="add Todo" />
			</form>
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
