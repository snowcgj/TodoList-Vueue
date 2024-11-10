<template>
  <li :class="{ completed: todo.completed }">
    <label>
      <input type="checkbox" :checked="todo.completed" @change="toggle">
      <span>{{ todo.text }}</span>
    </label>
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
li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 20px;
  border-bottom: 1px solid #ddd;
  background-color: white;
  transition: background-color 0.3s;
}

li:hover {
  background-color: #f1f1f1;
}

.completed span {
  text-decoration: line-through;
  color: gray;
}

label {
  display: flex;
  align-items: center;
  flex-grow: 1;
}

input[type="checkbox"] {
  margin-right: 10px;
  width: 18px;
  height: 18px;
}

button {
  background-color: #e74c3c;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #c0392b;
}
</style>
