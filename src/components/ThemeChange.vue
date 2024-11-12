<template>
<div class="theme-toggle">
      <button @click="toggleTheme">
        {{ theme === 'light' ? '🌙 Dark Mode' : '☀️ Light Mode' }}
      </button>
    </div>
</template>


<script>
// 这里的default就是默认导出，
//import ThemeChange from './ThemeChange.vue'; 这个是default
//import { ThemeChange } from './ThemeChange.vue';  这个就是export ThemeChange={}这样的导出  
export default{
    name:"ThemeChange",
    props: {  // props就是从父组件拿到这个进行使用
        theme: {
            type: String,
            required: true, // 主题状态必须从父组件传入
            },
    },
    methods:{
        toggleTheme(){
            this.$emit("update-theme", this.theme === "light" ? "dark" : "light");
        }
    }
}

</script>


<style>

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
/* 这里字体的颜色是继承，就和Todolist的字体颜色一样了 */
</style>