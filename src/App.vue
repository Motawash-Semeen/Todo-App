<script setup>
import Swal from "sweetalert2";
import { computed, onMounted, ref } from "vue";

const nameValue = ref(""); // nameHandel is a reactive variable
const data = localStorage.getItem("todoList")
  ? JSON.parse(localStorage.getItem("todoList"))
  : [];
const todoLists = ref(data); // todoList is a reactive variable
const todoData = ref({
  title: null,
  category: null,
}); // todoList is a reactive variable

// const revTodoList = computed(() => todoLists.value.sort((a,b) =>{
// 	return b.id - a.id
// }))

const revTodoList = computed(() => [...todoLists.value].reverse());

function nameDataHandel() {
  console.log(nameValue.value);
  localStorage.setItem("name", nameValue.value);
}

function handelFormSubmit() {
  if (!todoData.value.title || !todoData.value.category) {
    Swal.fire({
      icon: "error",
      title: "Oops...",
      text: "Please fill all the fields!",
    });
    todoData.value = {
      title: null,
      category: null,
    };
    return;
  }
  todoLists.value.push({
    id: todoLists.value.length + 1,
    title: todoData.value.title,
    category: todoData.value.category,
    isDone: false,
  });
  todoData.value = {
    title: null,
    category: null,
  };
  console.log(todoLists.value);
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

function handelDelete(id) {
  todoLists.value = todoLists.value.filter((todo) => todo.id !== id);
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

function handelDone(id) {
  const index = todoLists.value.findIndex((todo) => todo.id === id);
  todoLists.value[index].isDone = !todoLists.value[index].isDone;
  localStorage.setItem("todoList", JSON.stringify(todoLists.value));
}

onMounted(() => {
  document.getElementById("name").value = localStorage.getItem("name") || "";
});
</script>

<template>
  <main class="app">
    <section class="greeting">
      <h2 class="title">
        What's up,
        <input
          type="text"
          id="name"
          placeholder="Name here"
          v-model="nameValue"
          @keyup="nameDataHandel"
        />
      </h2>
    </section>

    <section class="create-todo">
      <h3>CREATE A TODO</h3>

      <form id="new-todo-form" @submit.prevent="handelFormSubmit">
        <h4>What's on your todo list?</h4>
        <input
          type="text"
          name="content"
          id="content"
          placeholder="e.g. make a video"
          v-model="todoData.title"
        />

        <h4>Pick a category</h4>
        <div class="options">
          <label>
            <input
              type="radio"
              name="category"
              id="category1"
              value="business"
              v-model="todoData.category"
            />
            <span class="bubble business"></span>
            <div>Business</div>
          </label>

          <label>
            <input
              type="radio"
              name="category"
              id="category2"
              value="personal"
              v-model="todoData.category"
            />
            <span class="bubble personal"></span>
            <div>Personal</div>
          </label>
        </div>

        <input type="submit" value="Add todo" />
      </form>
    </section>

    <section class="todo-list" v-if="todoLists.length > 0">
      <h3>TODO LIST</h3>
      <div class="list" id="todo-list">
        <div
          :class="{ 'todo-item': true, done: todoList.isDone }"
          v-for="todoList in revTodoList"
          :key="todoList.id"
        >
          <label>
            <input
              type="checkbox"
              :checked="todoList.isDone"
              @click="handelDone(todoList.id)"
            />
            <span
              :class="`bubble ${
                todoList.category == 'business' ? 'business' : 'personal'
              }`"
            ></span>
          </label>

          <div class="todo-content">
            <input type="text" v-model="todoList.title" />
          </div>

          <div class="actions">
            <button class="delete" @click="handelDelete(todoList.id)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
</style>
