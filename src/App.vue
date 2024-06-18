<script setup>
import { onMounted, ref } from 'vue'

const nameValue = ref(''); // nameHandel is a reactive variable
const todoList = ref([]); // todoList is a reactive variable
const todoData = ref({
	'title' : null,
	'category' : null,
}); // todoList is a reactive variable

function nameDataHandel(){
	console.log(nameValue.value);
	localStorage.setItem('name', nameValue.value);
}

function handelFormSubmit(){
	todoList.value.push(todoData.value);
	todoData.value = {
		'title' : null,
		'category' : null,
	};
	console.log(todoList.value);
	localStorage.setItem('todoList', JSON.stringify(todoList.value));
}

onMounted(() => {
	document.getElementById('name').value = localStorage.getItem('name') || '';
})

</script>

<template>
<main class="app">
		
		<section class="greeting">
			<h2 class="title">
				What's up, <input type="text" id="name" placeholder="Name here" v-model="nameValue" @keyup="nameDataHandel">
			</h2>
		</section>

		<section class="create-todo">
			<h3>CREATE A TODO</h3>

			<form id="new-todo-form" @submit.prevent="handelFormSubmit" >
				<h4>What's on your todo list?</h4>
				<input 
					type="text" 
					name="content" 
					id="content" 
					placeholder="e.g. make a video" v-model="todoData.title"/>
				
				<h4>Pick a category</h4>
				<div class="options">

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category1" 
							value="business" v-model="todoData.category"/>
						<span class="bubble business"></span>
						<div>Business</div>
					</label>

					<label>
						<input 
							type="radio" 
							name="category" 
							id="category2" 
							value="personal" v-model="todoData.category"/>
						<span class="bubble personal"></span>
						<div>Personal</div>
					</label>

				</div>

				<input type="submit" value="Add todo" />
			</form>
		</section>

		<section class="todo-list">
			<h3>TODO LIST</h3>
			<div class="list" id="todo-list">

				<div :class="`todo-item done`">
					<label>
						<input type="checkbox" />
						<span :class="`bubble business`"></span>
					</label>

					<div class="todo-content">
						<input type="text" value="test 1"/>
					</div>

					<div class="actions">
						<button class="delete">Delete</button>
					</div>
				</div>
				<div :class="`todo-item`">
					<label>
						<input type="checkbox" />
						<span :class="`bubble personal`"></span>
					</label>

					<div class="todo-content">
						<input type="text" value="test 2"/>
					</div>

					<div class="actions">
						<button class="delete">Delete</button>
					</div>
				</div>

			</div>
		</section>

	</main>
</template>

<style scoped>

</style>
