<template>
    <div class="import-export">
      <button @click="exportTodos">导出事项</button>
      <label for="import-file" class="import-button">导入事项</label>
      <input id="import-file" type="file" @change="importTodos" accept=".json" />
    </div>
</template>
  
<script>
  export default {
    name: 'ImportExport',
    methods: {
      exportTodos() {
        this.$emit('export-todos');
      },
      importTodos(event) {
        const file = event.target.files[0];
        if (!file) return;
  
        const reader = new FileReader();
        //FileReader 是一个用于读取文件内容的异步 API，可以读取文件为文本、数据 URL 或二进制等。
        reader.onload = (e) => { //文件读取成功后触发的事件处理函数
          try {
            const importedTodos = JSON.parse(e.target.result);
            if (Array.isArray(importedTodos)) {
              // 触发导入事件，并传递导入的todos
              this.$emit('import-todos', importedTodos);
              alert('导入成功！');
            } else {
              throw new Error('文件格式不正确');
            }
          } catch (error) {
            alert('导入失败：' + error.message);
          }
        }; 
        reader.readAsText(file);  // 这个执行完之后才会触发reader.onload函数执行
        // 重置文件输入
        event.target.value = '';
      }
    }
    //  这里面的event e  事件对象 需要重点理解一下
  };
</script>
  
  
<style scoped>
  .import-export {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 100px;
  }
  
  .import-export button,
  .import-export .import-button {
    padding: 10px 20px;
    background-color: #34c759;
    color: white;
    border: none;
    border-radius: 15px;
    cursor: pointer;
    font-size: 14px;
    transition: background-color 0.3s, transform 0.2s;
    margin-right: 10px;
  }
  
  .import-export button:hover {
    background-color: #28a745;
  }
  
  .import-export .import-button {
    background-color: #ff9500;
  }
  
  .import-export .import-button:hover {
    background-color: #cc7a00;
  }
  
  .import-export input[type="file"] {
    display: none;
  }

  .app-container.dark button{
    background-color: #ff9500;
  }
  .app-container.dark .import-button{
    background-color: #28a745;
  }
  
  </style>
  