<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const name = ref('')

const input_content = ref('')

const filter = ref('all')


const todos_asc = computed(() => todos.value.sort((a,b) =>{
	return a.createdAt - b.createdAt
}))

watch(name, (newVal) => {
	localStorage.setItem('name', newVal)
})

watch(todos, (newVal) => {
	localStorage.setItem('todos', JSON.stringify(newVal))
}, {
	deep: true
})

const filteredTodos = computed(() => {
    switch (filter.value) {
        case 'complete':
            return todos.value.filter(todo => todo.done);
        case 'notComplete':
            return todos.value.filter(todo => !todo.done);
        default:
            return todos.value;
    }
});

const addTodo = () => {
	if (input_content.value.trim() === '') {
		return
	}

	todos.value.push({
		content: input_content.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}

const removeTodo = (todo) => {
	todos.value = todos.value.filter((t) => t !== todo)
}

onMounted(() => {
	name.value = localStorage.getItem('name') || ''
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})
</script>

<template>
    <main class="app">
        <section class="greeting">
            <h2 class="title">
                Hai welcome to, <input type="text" id="name" placeholder="Name here" v-model="name"> Note
            </h2>
        </section>

        <section class="create-todo">
            <h3>CREATE ACTIVITIES</h3>
            <form id="new-todo-form" @submit.prevent="addTodo">
                <h4>What's on your activities?</h4>
                <input type="text" id="content" placeholder="Add your activities" v-model="input_content" />
                <input type="submit" value="Add todo" />
            </form>
        </section>
    </main>

    <main class="app2">
        <section class="todo-list">
            <h3>Your Activities</h3>
            <div>
                <!-- Filter Buttons -->
                <button @click="filter = 'all'"> All </button>
				<button @click="filter = 'complete'"> Complete </button>
                <button @click="filter = 'notComplete'"> Not Complete </button>
                
            </div>
            <div class="list" id="todo-list">
                <div v-for="todo in filteredTodos" :key="todo.createdAt" :class="`todo-item ${todo.done ? 'done' : ''}`">
                    <label>
                        <input type="checkbox" v-model="todo.done" />
                        <span class="bubble"></span>
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
    </main>
</template>

<style>
:root {
	--primary: #102C57;
	--color1: #fbcd9b;
	--light: #FEFAF6;
	--grey: #f0d7b5;
	--dark: #313154;
	--danger: #ff5b57;

	--shadow: 0 1px 3px rgba(0, 0, 0, 0.1);

	--color1-glow: 0px 0px 4px #f4d5b3;
	--primary-glow: 0px 0px 4px #102C57;
}

* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'montserrat', sans-serif;
}

input:not([type="checkbox"]):not([type="checkbox"]), button {
	appearance: none;
	border: none;
	outline: none;
	background: none;
	cursor: initial;
}

.app {
	width: 100%;
	max-width: 850px;
	background: var(--light);
	margin: 100px auto 20px;
	padding: 40px 30px 70px;
	border-radius: 10px;
}

.app2 {
	width: 100%;
	max-width: 850px;
	background: var(--light);
	margin: 10px auto 20px;
	padding: 10px 30px 70px;
	border-radius: 10px;
}

body {
	background: var(--color1);
	color: var(--dark);
}

section {
	margin-top: 2rem;
	margin-bottom: 2rem;
	padding-left: 1.5rem;
	padding-right: 1.5em;
}

h3 {
	color: var(--dark);
	font-size: 1rem;
	font-weight: 400;
	margin-bottom: 0.5rem;
}

h4 {
	color: var(--grey);
	font-size: 0.875rem;
	font-weight: 700;
	margin-bottom: 0.5rem;
}

.greeting .title {
	display: flex;
}

.greeting .title input {
	margin-left: 0.5rem;
	flex: 1 1 0%;
	min-width: 0;
}

.greeting .title,
.greeting .title input {
	color: var(--dark);
	font-size: 1.5rem;
	font-weight: 700;
}

.create-todo input[type="text"] {
	display: block;
	width: 100%;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: var(--dark);
	background-color: #FFF;
	border-radius: 0.5rem;
	box-shadow: var(--shadow);
	margin-bottom: 1.5rem;
}

.create-todo .options {
	display: grid;
	grid-template-columns: repeat(2, 1fr);
	grid-gap: 1rem;
	margin-bottom: 1.5rem;
}

.create-todo .options label {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	padding: 1.5rem;
	background-color: #FFF;
	border-radius: 0.5rem;
	box-shadow: var(--shadow);
	cursor: pointer;
}

button {
	border: none solid var(--color1);
	box-shadow: var(--color1-glow);
	display: inline;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	padding: 4px;
	background-color: #fff7e3;
	border-radius: 3px;
	cursor: pointer;
	border: #fff7e3;
	border-radius: 20%;
}

input[type="checkbox"],
input[type="checkbox"] {
	display: none;
}

.bubble {
	display: flex;
	align-items: center;
	justify-content: center;
	width: 20px;
	height: 20px;
	border-radius: 0%;
	border: 3px solid var(--color1);
	box-shadow: var(--color1-glow);
}

.bubble::after {
	content: "✔️";
	align-items: center;
	display: flex;
	opacity: 0;
	width: 10px;
	height: 0px;
	background-color: var(--color1);
	box-shadow: var(--color1-glow);

}

input:checked ~ .bubble::after {
	width: 10px;
	height: 10px;
	opacity: 1;
}

.create-todo .options label div {
	color: var(--dark);
	font-size: 1.125rem;
	margin-top: 1rem;
}

.create-todo input[type="submit"] {
	display: block;
	width: 100%;
	font-size: 1.125rem;
	padding: 1rem 1.5rem;
	color: #FFF;
	background-color: var(--primary);
	border-radius: 0.5rem;
	box-shadow: var(--primary-glow);
	cursor: pointer;
	transition: 0.2s ease-in-out;
}

.create-todo input[type="submit"]:hover {
	opacity: 0.75;
}

.todo-list .list {
	margin: 1rem 0;
} 

.todo-list .todo-item {
	display: flex;
	align-items: center;
	background-color: #FFF;
	padding: 1rem;
	border-radius: 0.5rem;
	box-shadow: var(--shadow);
	margin-bottom: 1rem;
}

.todo-item label {
	display: block;
	margin-right: 1rem;
	cursor: pointer;
}

.todo-item .todo-content {
	flex: 1 1 0%;
}

.todo-item .todo-content input {
	color: var(--dark);
	font-size: 1.125rem;
}

.todo-item .actions {
	display: flex;
	align-items: center;
}

.todo-item .actions button {
	display: block;
	padding: 0.5rem;
	border-radius: 0.25rem;
	color: #FFF;
	cursor: pointer;
	transition: 0.2s ease-in-out;
}

.todo-item .actions button:hover {
	opacity: 0.75;
}

.todo-item .actions .edit {
	margin-right: 0.5rem;
	background-color: var(--primary);
}

.todo-item .actions .delete {
	background-color: var(--danger);
}

.todo-item.done .todo-content input {
	text-decoration: line-through;
	color: var(--grey);
}
</style>