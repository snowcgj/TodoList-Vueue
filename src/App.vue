<template>
  <div id="app">
    <h1>Todo List</h1>

    <div class="input-section">
      <input
        v-model="newTodo"
        @keyup.enter="addTodo"
        placeholder="添加新的Todo"
      />
      <button @click="addTodo">添加</button>
    </div>

    <div class="filters">
      <button
        :class="{ active: filter === 'all' }"
        @click="filter = 'all'"
      >
        全部
      </button>
      <button
        :class="{ active: filter === 'active' }"
        @click="filter = 'active'"
      >
        未完成
      </button>
      <button
        :class="{ active: filter === 'completed' }"
        @click="filter = 'completed'"
      >
        已完成
      </button>
    </div>

    <transition-group name="list" tag="ul">
      <todo-item
        v-for="todo in filteredTodos"
        :key="todo.id"
        :todo="todo"
        @delete="deleteTodo"
        @toggle="toggleTodo"
      ></todo-item>
    </transition-group>

    <div v-if="todos.length" class="clear-completed">
      <button @click="clearCompleted">清除已完成的任务</button>
    </div>
  </div>
</template>

<script>
import TodoItem from './components/TodoItem.vue';

export default {
  name: 'App',
  components: {
    TodoItem
  },
  data() {
    return {
      newTodo: '',
      todos: [],
      filter: 'all' // 新增过滤器
    };
  },
  computed: {
    filteredTodos() {
      if (this.filter === 'active') {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter === 'completed') {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return;
      this.todos.push({
        id: Date.now(),
        text: this.newTodo,
        completed: false
      });
      this.newTodo = '';
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    toggleTodo(id) {
      const todo = this.todos.find(todo => todo.id === id);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    }
  }
};
</script>

<style>
#app {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #333;
}

.input-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  width: 70%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  transition: border-color 0.3s;
}

input:focus {
  border-color: #5dade2;
  outline: none;
}

button {
  padding: 10px 20px;
  margin-left: 10px;
  background-color: #5dade2;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #3498db;
}

.filters {
  margin-bottom: 20px;
}

.filters button {
  margin: 0 5px;
  background-color: #e7e7e7;
  color: #333;
}

.filters button.active {
  background-color: #5dade2;
  color: white;
}

ul {
  list-style-type: none;
  padding: 0;
  min-height: 200px;
}

.list-enter-active, .list-leave-active {
  transition: all 0.5s;
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

.clear-completed {
  margin-top: 20px;
}

.clear-completed button {
  background-color: #e74c3c;
}

.clear-completed button:hover {
  background-color: #c0392b;
}
</style>
