<template>
  <div id="app">
    <h1>Todo List</h1>

    <input
      v-model="newTodo"
      @keyup.enter="addTodo"         
      placeholder="添加新的Todo"
    />
    <!-- @keyup.enter  @就是on的缩写  等待键盘 enter事件 -->

    <button @click="addTodo">添加</button>
    <!-- click keyup.enter 都是IO事件 -->

    <ul>
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @delete="deleteTodo"
        @toggle="toggleTodo"
      ></todo-item>
      <!-- :key  就是动态绑定，怎么理解呢，就是现在给这个组件创造了一个属性，其他地方可能用得到 -->
      <!-- 子组件 <todo-item> 可以通过 $emit 方法触发这些自定义事件。 -->
    </ul>

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
      newTodo: '',  // 在上面绑定着呢
      todos: []
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return; //.trim 会自动移除开头结尾的空格 
      //  === 表示严格相等， ‘5’==5 这种在js中是正确的， ‘5’===5 就是错误的了
      this.todos.push({
        id: Date.now(),
        text: this.newTodo,
        completed: false
      });  //push进新数据
      this.newTodo = ''; // 清空行
    },

    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },

    toggleTodo(id) {
      const todo = this.todos.find(todo => todo.id === id);
      if (todo) {
        todo.completed = !todo.completed;
      }
    }
  }
};
</script>

<style>
#app {
  max-width: 500px;
  margin: 50px auto;
  text-align: center;
}
input {
  width: 70%;
  padding: 10px;
  margin-right: 10px;
}
button {
  padding: 10px 20px;
}
ul {
  list-style-type: none;
  padding: 0;
}
</style>