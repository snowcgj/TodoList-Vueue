<template>
    <li :class="{ completed: todo.completed }">
      <label class="checkbox-wrapper">
        <input type="checkbox" :checked="todo.completed" @change="toggle">
        <span class="checkmark"></span>
        <span class="todo-text">{{ todo.text }}</span>
      </label>
      <div class="button-container">
        <button @click="deleteItem">删除</button>
      </div>
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
  li {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    border-bottom: 1px solid #d1d1d6;
    background-color: #ffffff;
    transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
    border-radius: 10px;
    margin-bottom: 10px;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  }
  
  .app-container.dark li {
    background-color: #2c2c2e;
    border: 1px solid #444;
    box-shadow: 0 1px 3px rgba(255, 255, 255, 0.1);
  }
  
  li:hover {
    background-color: #f9f9f9;
    transform: translateY(-2px);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15);
  }
  
  .app-container.dark li:hover {
    background-color: #3a3a3c;
    box-shadow: 0 4px 6px rgba(255, 255, 255, 0.15);
  }
  
  .completed span.todo-text {
    text-decoration: line-through;
    color: #8e8e93;
  }
  
  .app-container.dark .completed span.todo-text {
    color: #a1a1a6;
  }
  
  .checkbox-wrapper {
    display: flex;
    align-items: center;
    flex: 1;
    cursor: pointer;
    position: relative;
  }
  
  .checkbox-wrapper input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    cursor: pointer;
    height: 24px;
    width: 24px;
    margin: 0;
  }
  
  .checkmark {
    position: relative;
    height: 24px;
    width: 24px;
    background-color: #f2f2f7;
    border-radius: 6px;
    margin-right: 15px;
    transition: background-color 0.3s;
  }
  
  .checkbox-wrapper input:checked ~ .checkmark {
    background-color: #007aff;
  }
  
  .app-container.dark .checkbox-wrapper input:checked ~ .checkmark {
    background-color: #0a84ff;
  }
  
  .checkmark::after {
    content: "";
    position: absolute;
    display: none;
  }
  
  .checkbox-wrapper input:checked ~ .checkmark::after {
    display: block;
  }
  
  .checkbox-wrapper .checkmark::after {
    left: 8px;
    top: 4px;
    width: 6px;
    height: 12px;
    border: solid white;
    border-width: 0 2px 2px 0;
    transform: rotate(45deg);
  }
  
  .todo-text {
    flex: 1;
    text-align: left;
    word-break: break-word; /* 防止长文本溢出 */
  }
  
  .button-container {
    flex: 0 0 60px; /* 固定宽度 */
    display: flex;
    justify-content: flex-end;
  }
  
  button {
    background-color: #ff3b30;
    color: white;
    border: none;
    padding: 8px 14px;
    border-radius: 12px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
    font-size: 14px;
  }
  
  button:hover {
    background-color: #c12720;
  }
  
  button:active {
    transform: scale(0.95);
  }
  
  .app-container.dark button {
    background-color: #ff453a;
  }
  
  .app-container.dark button:hover {
    background-color: #c12720;
  }
  </style>
  