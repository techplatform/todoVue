<script setup>
	import { ref, onMounted, computed, watch } from 'vue'

	const todoitem = ref([])
	const user_input = ref('')



	const todoitem_asc = computed(() => todoitem.value.sort((a,b) =>{
		return a.createdAt - b.createdAt
	}))
	const addItem = () => {
		if (user_input.value.trim() === '') {
			return
		}
		todoitem.value.push({
			content: user_input.value,
			
			done: false,
			
			createdAt: new Date().getTime()
		})

		user_input.value = ''
	}

	const removeTodo = todo => {
		todoitem.value = todoitem.value.filter(t => t !==todo)
	}

	watch(todoitem, (newVal) => {
		localStorage.setItem('todoitem', JSON.stringify(newVal))
	}, {
		deep: true
	})

	onMounted(() => {
		todoitem.value = JSON.parse(localStorage.getItem('todoitem')) || []
	})
</script>

<template>
	<main class="app">
		
		<section class="firstMSG">
			<h2 class="title">
				Hey There!
			</h2>
		</section>

		<section class="createTodo">
			<h3>What's on your todo list?</h3>

			<form id="new-todo-form" @submit.prevent="addItem">

				<input type="text" name="content" id="content" placeholder="ex. Get Milk" v-model="user_input" />

				<input type="submit" value="Add todo" />
				
			</form>
		</section>

		<section class="TodoList">
			<h3>TODO LIST</h3>
			<div class="list" id="TodoList">

				<div v-for="todo in todoitem_asc" :class="`TodoItem ${todo.done && 'done'}`">
					<label>
						<input type="checkbox" v-model="todo.done" />
            			<span :class="`bullet ${ todo.category == 'marked' }`"></span>
					</label>

					<div class="todo-content">
						<input type="text" v-model="todo.content" />
						
					</div>

					<div class="delaction">
						<button class="delete" @click="removeTodo(todo)"><font-awesome-icon icon="fa-solid fa-trash-can" /></button>
					</div>

				</div>

			</div>
		</section>

	</main>
</template>