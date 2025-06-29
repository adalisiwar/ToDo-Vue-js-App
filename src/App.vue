<template>
  <div class="todo-app" v-if="todos.length === 0">No todos</div>
  <div class="todo-app" v-else>
    <h1>To-Do List</h1>
    <form @submit.prevent="addTodo">
      <input
        v-model="newTodo"
        type="text"
        placeholder="Add a new task"
        autocomplete="off"
      />
      <button :disabled="newTodo.length === 0" type="submit">Add</button>
    </form>
    <label>
      <input type="checkbox" v-model="hideCompleted" />
      Hide completed tasks
    </label>
    <ul>
      <li
        v-for="todo in filteredTodos"
        :key="todo.createdAt"
        :class="{ completed: todo.completed }"
      >
        <label>
          <input type="checkbox" v-model="todo.completed" />
          {{ todo.text }}
        </label>
        <button @click="deleteTodo(todo.createdAt)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const todos = ref([]);
const newTodo = ref("");
const hideCompleted = ref(false);

const addTodo = () => {
  const task = newTodo.value.trim();
  if (task) {
    todos.value.push({
      text: task,
      completed: false,
      createdAt: Date.now(),
    });
    newTodo.value = "";
  }
};

const deleteTodo = (createdAt) => {
  todos.value = todos.value.filter((todo) => todo.createdAt !== createdAt);
};

const filteredTodos = computed(() => {
  if (hideCompleted.value) {
    return todos.value.filter((todo) => !todo.completed);
  }
  return todos.value;
});
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  opacity: 0.5;
}
.todo-app {
  max-width: 400px;
  margin: 2rem auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

form {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

input[type="text"] {
  flex-grow: 1;
  padding: 0.5rem;
  font-size: 1rem;
}

button {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  cursor: pointer;
}

ul {
  list-style-type: none;
  padding-left: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.3rem 0;
  border-bottom: 1px solid #ccc;
}

li button {
  background-color: #ff4d4d;
  border: none;
  color: white;
  border-radius: 3px;
}
</style>
