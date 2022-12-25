<template>
	<div class="flex flex-col items-center">
		<h1 class="text-4xl font-bold mb-4">MyTodoList</h1>
		<form class="w-full max-w-sm" @submit.prevent="addTodo">
			<div class="flex items-center border-b-2 border-teal-500 py-2">
				<input
					class="appearance-none bg-transparent border-none w-full text-gray-700 mr-3 py-1 px-2 leading-tight focus:outline-none"
					type="text"
					v-model="newTodo"
					placeholder="Add a todo"
				/>
				<button
					class="flex-shrink-0 bg-teal-500 hover:bg-teal-700 border-teal-500 hover:border-teal-700 text-sm border-4 text-white py-1 px-2 rounded"
					type="submit"
				>
					Add
				</button>
			</div>
		</form>
		<ul class="w-full max-w-sm mt-4">
			<li
				v-for="todo in todos"
				:key="todo.id"
				@click="toggleTodo(todo)"
				:class="{ 'todo-done': todo.done }"
				class="flex items-center justify-between mb-4 p-4 rounded-lg shadow-lg bg-white"
			>
				{{ todo.text }}
				<button
					@click="removeTodo(todo.id)"
					class="text-red-500 hover:text-red-700"
				>
					X
				</button>
			</li>
		</ul>
	</div>
</template>

<script>
export default {
	data() {
		return {
			newTodo: '',
			todos: [],
		};
	},
	created() {
		this.fetchTodos();
	},
	methods: {
		fetchTodos() {
			// Load the todos from the local storage
			const todos = JSON.parse(localStorage.getItem('todos')) || [];
			this.todos = todos;
		},
		saveTodos() {
			// Save the todos to the local storage
			localStorage.setItem('todos', JSON.stringify(this.todos));
		},
		addTodo() {
			this.todos.push({
				id: Date.now(), // Use the current timestamp as the id
				text: this.newTodo,
				done: false,
			});
			this.newTodo = '';
			this.saveTodos();
		},
		toggleTodo(todo) {
			todo.done = !todo.done;
			this.saveTodos();
		},
		removeTodo(id) {
			this.todos = this.todos.filter((todo) => todo.id !== id);
			this.saveTodos();
		},
	},
};
</script>

<style src="..\style.css"></style>
