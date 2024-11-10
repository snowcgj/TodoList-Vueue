<template>
  <div :class="['app-container', theme]">
    <h1>Todo List</h1>

    <div class="theme-toggle">
      <button @click="toggleTheme">
        {{ theme === 'light' ? '🌙 Dark Mode' : '☀️ Light Mode' }}
      </button>
    </div>

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

    <transition-group name="list" tag="ul" class="todo-list">
      <todo-item
        v-for="todo in sortedTodos"
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
      filter: 'all',
      theme: 'light' // 默认主题
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
    },
    sortedTodos() {
      // 创建 filteredTodos 的副本并排序，避免副作用
      return [...this.filteredTodos].sort((a, b) => {
        // 未完成的在前，已完成的在后
        if (a.completed !== b.completed) {
          return a.completed ? 1 : -1;
        }

        if (!a.completed && !b.completed) {
          // 未完成的按创建时间降序
          return new Date(b.createdAt) - new Date(a.createdAt);
        }

        if (a.completed && b.completed) {
          // 已完成的按完成时间降序
          return new Date(b.completedAt) - new Date(a.completedAt);
        }

        return 0;
      });
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return;
      this.todos.push({
        id: Date.now(),
        text: this.newTodo,
        completed: false,
        createdAt: new Date(),      // 记录创建时间
        completedAt: null           // 完成时间初始为null
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
        todo.completedAt = todo.completed ? new Date() : null; // 记录完成时间或重置
      }
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    toggleTheme() {
      this.theme = this.theme === 'light' ? 'dark' : 'light';
    }
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=SF+Pro+Text:wght@400;600&display=swap');

.app-container {
  max-width: 600px;
  margin: 50px auto;
  text-align: center;
  font-family: 'SF Pro Text', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  padding: 20px 25px;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s, color 0.3s;
}

.app-container.light {
  background-color: #ffffff;
  color: #1c1c1e;
}

.app-container.dark {
  background-color: #1c1c1e;
  color: #ffffff;
  box-shadow: 0 10px 20px rgba(255, 255, 255, 0.1);
}

h1 {
  color: inherit;
  font-size: 2em;
  margin-bottom: 20px;
}

.theme-toggle {
  text-align: right;
  margin-bottom: 10px;
}

.theme-toggle button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
  color: inherit;
}

.input-section {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.input-section input {
  flex: 1;
  padding: 12px 15px;
  border: 1px solid #d1d1d6;
  border-radius: 15px;
  font-size: 16px;
  background-color: #f2f2f7;
  transition: border-color 0.3s, box-shadow 0.3s;
}

.app-container.dark .input-section input {
  background-color: #2c2c2e;
  border: 1px solid #444;
  color: #ffffff;
}

.input-section input:focus {
  border-color: #007aff;
  box-shadow: 0 0 5px rgba(0, 122, 255, 0.5);
  outline: none;
}

.input-section button {
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

.input-section button:hover {
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

.app-container.dark .filters button {
  background-color: #2c2c2e;
  color: #ffffff;
}

.filters button.active {
  background-color: #007aff;
  color: white;
}

.filters button:hover:not(.active) {
  background-color: #e5e5ea;
}

.app-container.dark .filters button:hover:not(.active) {
  background-color: #3a3a3c;
}

.todo-list {
  list-style-type: none;
  padding: 0;
  height: 400px; /* 固定高度，保持一致 */
  overflow-y: auto;  /* 启用垂直滚动 */
  overflow-x: hidden; /* 禁用水平滚动 */
}

/* 自定义滚动条样式 */
.todo-list::-webkit-scrollbar {
  width: 8px;
}

.todo-list::-webkit-scrollbar-track {
  background: #f1f1f7;
  border-radius: 4px;
}

.todo-list::-webkit-scrollbar-thumb {
  background: #c1c1c6;
  border-radius: 4px;
}

.app-container.dark .todo-list::-webkit-scrollbar-track {
  background: #2c2c2e;
}

.app-container.dark .todo-list::-webkit-scrollbar-thumb {
  background: #555;
}

.list-enter-active, .list-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.list-enter-from, .list-leave-to {
  opacity: 0;
  transform: translateY(-10px);
}

.list-leave-active {
  transition: opacity 0.5s, transform 0.5s;
}

.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

.clear-completed {
  margin-top: 20px;
}

.clear-completed button {
  background-color: #ff3b30;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 15px;
  cursor: pointer;
  font-size: 14px;
  transition: background-color 0.3s, transform 0.2s;
}

.clear-completed button:hover {
  background-color: #c12720;
}

.app-container.dark .clear-completed button {
  background-color: #ff453a;
}

.app-container.dark .clear-completed button:hover {
  background-color: #c12720;
}
</style>
