<template>
    <div class="daily-quote">
      <p>{{ quote }}</p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'DailyQuote',
    data() {
      return {
        quote: '保持简单，保持专注。' // 默认每日一言
      };
    },
    methods: {
      fetchQuote() {
        //fetch() 是一个 JavaScript 内置的函数，用于执行网络请求，默认返回一个 Promise。
        fetch('https://v1.hitokoto.cn/')
          .then(response => response.json())
          .then(data => {
            this.quote = `${data.hitokoto} —— ${data.from}`;
          })
          .catch(() => {
            // 如果API请求失败，保持默认每日一言
            this.quote = '保持简单，保持专注。';
          });
      }
    },
    mounted() {
      this.fetchQuote();
    }//  表示一旦组件挂载成功，就调用 fetchQuote() 方法来获取每日一句。
  };
  </script>
  
  <style scoped>
  .daily-quote {
    margin-bottom: 20px;
    font-style: italic;
    color: #555;
    border-left: 4px solid #007aff;
    padding-left: 10px;
    transition: color 0.3s, border-left-color 0.3s;
  }
  
  
  .app-container.dark    .daily-quote {
    color: #a1a1a6;
    border-left: 4px solid #0fe0e4;
  }

  /* ，
  当 .daily-quote 组件位于一个具有 .app-container 和 .dark 类的父元素中时，应用特定的样式。这通常用于实现暗黑模式（Dark Mode）或主题切换功能，使得在不同的主题下，组件的外观有所变化。 
  设置css主题时  用这个
  */

  </style>
  