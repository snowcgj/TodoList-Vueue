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
@import url('https://fonts.googleapis.com/css2?family=San+Francisco:wght@400;600&display=swap');

#app {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
  font-family: 'San Francisco', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  background-color: #ffffff;
  padding: 20px 25px;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

h1 {
  color: #1c1c1e;
  font-size: 2em;
  margin-bottom: 20px;
}

.input-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 12px 15px;
  border: 1px solid #d1d1d6;
  border-radius: 15px;
  font-size: 16px;
  background-color: #f2f2f7;
  transition: border-color 0.3s, box-shadow 0.3s;
}

input:focus {
  border-color: #007aff;
  box-shadow: 0 0 5px rgba(0, 122, 255, 0.5);
  outline: none;
}

button {
  padding: 12px 20px;
  margin-left: 10px;
  background-color: #007aff;
  color: white;
  border: none;
  border-radius: 15px;
  cursor: pointer;
  font-size: 16px;
  transition: background-color 0.3s, transform 0.2s;
}

button:hover {
  background-color: #0051a8;
}

.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.filters button {
  margin: 0 5px;
  background-color: #f2f2f7;
  color: #1c1c1e;
  border: none;
  padding: 8px 15px;
  border-radius: 15px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
  font-size: 14px;
}

.filters button.active {
  background-color: #007aff;
  color: white;
}

.filters button:hover:not(.active) {
  background-color: #e5e5ea;
}

ul {
  list-style-type: none;
  padding: 0;
  min-height: 200px;
}

.list-enter-active, .list-leave-active {
  transition: all 0.3s ease;
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.clear-completed {
  margin-top: 20px;
}

.clear-completed button {
  background-color: #ff3b30;
}

.clear-completed button:hover {
  background-color: #c12720;
}
</style>

