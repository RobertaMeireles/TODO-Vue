<template>
	<div class="form-todo">
	<div class="logo">
		<img src="../assets/logo.png" alt="logo TODO"/>
	</div>
	<Date/>
	<form @submit.prevent="addTodo()">
		<input
			v-model="newTodo"
			name="newTodo"
			autocomplete="off"
			placeholder="Type your Best TODO"
		>
		<button class="btn-add"><i class="fas fa-plus"></i></button>

	</form>
	<h2>TODO List</h2>
	<ul>
		<li
			v-for="(todo, index) in todos"
			:key="index"
		>
			<p :class="{ done: todo.done }">{{ todo.content }} </p>
			<div>
				<a class="btn-edit" @click="editTodo(todo,index)"><i class="fas fa-pen-alt"></i></a>
				<a class="btn-remove" @click="removeTodo(index)"><i class="far fa-trash-alt"></i></a>
				<a class="btn-done" @click="doneTodo(todo)" v-if="todo.done"><i class="fas fa-times"></i></a>
				<a class="btn-done" @click="doneTodo(todo)" v-else><i class="fas fa-check"></i></a>
			</div>
		</li>
	</ul>
	<h4 v-if="todos.length === 0">Empty list.</h4>
	<a class="btn-clear" @click="clearAll()">Clear List</a>
	</div>
</template>

<script>
	import { ref } from 'vue';
	import Date from './Date'
	export default {
		name: 'Todo',
		components: {
			Date
		},

		setup () {
			const newTodo = ref('');
			const defaultData = [{
				done: false,
				content: 'Write a TODO'
			}]
			const todosData = JSON.parse(localStorage.getItem('todos')) || defaultData
			const todos = ref(todosData)
			function addTodo () {
				if (newTodo.value) {
					todos.value.push({
						done: false,
						content: newTodo.value
					});
					newTodo.value = '';
				}
				saveData();
			}
			function doneTodo (todo) {
				todo.done = !todo.done
				saveData()
			}
			function removeTodo (index) {
				todos.value.splice(index, 1)
				saveData();
			}
			function editTodo(todo,index) {
				if(todo.done === true) {
					alert('You are not allowed to change a completed TODO.')
				} else {
					let NewInfo = prompt("Please enter with your change:")
					if ( NewInfo == '') {
						alert('Text can not be blank.')
					} else if (NewInfo === null) {
						return
					}else {
						todos.value.content = todos.value[index]
						todos.value.content.content = NewInfo
						saveData();
					}
				}
			}
			function clearAll () {
				todos.value = []
				saveData()
			}
			function saveData () {
				const storageData = JSON.stringify(todos.value)
				localStorage.setItem('todos', storageData)
			}

			return {
				todos,
				newTodo,
				addTodo,
				doneTodo,
				removeTodo,
				editTodo,
				clearAll,
				saveData
			}
		}
	}
</script>
