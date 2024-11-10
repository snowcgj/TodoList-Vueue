<template>
  <li :class="{ completed: todo.completed }">
    <div class="checkbox-container">
      <input type="checkbox" :checked="todo.completed" @change="toggle">
    </div>
    <div class="text-container">
      <span>{{ todo.text }}</span>
    </div>
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
  justify-content: space-between;
  padding: 15px 20px;
  border-bottom: 1px solid #d1d1d6;
  background-color: #ffffff;
  transition: background-color 0.3s, transform 0.2s;
  border-radius: 10px;
  margin-bottom: 10px;
}

li:hover {
  background-color: #f9f9f9;
  transform: translateY(-2px);
}

.completed span {
  text-decoration: line-through;
  color: #8e8e93;
}

.checkbox-container {
  flex: 0 0 30px; /* 固定宽度，确保所有复选框对齐 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.text-container {
  flex: 1; /* 文本部分占据剩余空间 */
  padding: 0 10px; /* 为文本添加左右内边距，防止与复选框和按钮紧贴 */
  text-align: left;
}

.button-container {
  flex: 0 0 80px; /* 固定宽度，确保删除按钮对齐 */
  display: flex;
  justify-content: flex-end;
}

input[type="checkbox"] {
  width: 20px;
  height: 20px;
  accent-color: #007aff; /* iOS 风格的蓝色勾选框 */
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
</style>
