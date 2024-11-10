<template>
    <!-- 在对象 { completed: todo.completed } 中：
completed 是类名（CSS 类），它会在元素的 class 属性中动态应用。t
 odo.completed 是一个布尔值（即 true 或 false），决定是否添加这个类名。 -->
    <li :class="{ completed: todo.completed }">
      <input type="checkbox" :checked="todo.completed" @change="toggle">
      <span>{{ todo.text }}</span>
      <button @click="deleteItem">删除</button>
    </li>
  </template>
  
  <script>
  export default {
    name: 'TodoItem',
    props: ['todo'],
    methods: {
      deleteItem() {
        this.$emit('delete', this.todo.id);
      },
      toggle() {
        this.$emit('toggle', this.todo.id);
      }
    }
  };
  </script>
  
  <style scoped>
  .completed span {
    text-decoration: line-through;
    color: gray;
  }
  li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #ddd;
  }
  button {
    background-color: red;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
  }
  </style>
  