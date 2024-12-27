<template>

  <!-- 使用 Iconify 的 Icon 组件 -->
  <Icon icon="mdi:account" width="32" height="32" color="blue" />


  <div :class="['app-container', theme]">

    
    <h1>Todo                List</h1>

    <!-- 导入导出组件 -->
    <ImportExport @export-todos="exportTodos" @import-todos="handleImportTodos" />
    <!-- 每日一言组件 -->
    <DailyQuote />

    <ThemeChange :theme="theme" @update-theme="handleToggleTheme" />
    <!-- 修改为单独的一个组件   组件名字  动态属性  监听子组件的处理函数 -->
    <!-- <div class="theme-toggle">
      <button @click="toggleTheme">
        {{ theme === 'light' ? '🌙 Dark Mode' : '☀️ Light Mode' }}
      </button>
    </div> -->

    

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
        @update-notes="updateNotes"
      ></todo-item>
    </transition-group>

    <div v-if="todos.length" class="clear-completed">
      <button @click="clearCompleted">清除已完成的任务</button>
    </div>
  </div>
</template>

<script >
import {Icon} from '@iconify/vue';
import TodoItem from './components/TodoItem.vue';
import DailyQuote from './components/DailyQuote.vue';
import ImportExport from './components/ImportExport.vue';
import ThemeChange from "./components/ThemeChange.vue"; // 引入组件
import './css/App.css'; // 导入外部CSS



export default {
  name: 'App',
  components: {
    TodoItem,
    DailyQuote,
    ImportExport,
    ThemeChange,
    Icon
  },
  data() { //创建每个实例的时候，就会自动执行data()函数，然后得到一份数据的示例
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
        completedAt: null,          // 完成时间初始为null
        notes: ''                   // 添加备注字段
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
    },
    updateNotes(id, notes) {
      const todo = this.todos.find(todo => todo.id === id);
      if (todo) {
        todo.notes = notes;
      }
    },
    exportTodos() {
      const dataStr = JSON.stringify(this.todos, null, 2);
      const blob = new Blob([dataStr], { type: "application/json" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'todos.json';
      a.click();
      URL.revokeObjectURL(url);
    },

    handleToggleTheme(newTheme){
      this.theme = newTheme;
    },

    handleImportTodos(importedTodos) {
      // 验证导入的todos格式
      if (Array.isArray(importedTodos)) {
        this.todos = importedTodos.map(todo => ({
          ...todo,
          createdAt: new Date(todo.createdAt),
          completedAt: todo.completedAt ? new Date(todo.completedAt) : null
        }));
      } else {
        alert('导入的文件格式不正确');
      }
    }
  },
  mounted() {
    // 从本地存储加载todos（可选）
    const savedTodos = localStorage.getItem('todos');
    if (savedTodos) {
      try {
        this.todos = JSON.parse(savedTodos).map(todo => ({
          ...todo,
          createdAt: new Date(todo.createdAt),
          completedAt: todo.completedAt ? new Date(todo.completedAt) : null
        }));
      } catch (e) {
        console.error('加载todos失败', e);
      }
    }
  },
  watch: {  
    //这个的主要功能就是持久或存储  保存在浏览器中，即使刷新也没事
    //  被监听得数据：   hander这个名字只是一个习惯，可以使用其他名字，save,dd这样子都可以使用 （）就还是正常的函数参数
    todos: {
      handler(todos) {
        // 将todos保存到本地存储
        localStorage.setItem('todos', JSON.stringify(todos));
      },
      deep: true
    }
  }
};
</script>


