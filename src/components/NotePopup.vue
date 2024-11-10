<!-- src/components/NotePopup.vue -->
<template>
    <div v-if="visible" class="popup" @mouseleave="hide">
      <textarea v-model="note" placeholder="添加备注..." maxlength="200"></textarea>
      <button @click="save">保存</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'NotePopup',
    props: {
      initialNote: {
        type: String,
        default: ''
      }
    },
    data() {
      return {
        note: this.initialNote,
        visible: false
      };
    },
    methods: {
      show() {
        this.visible = true;
      },
      hide() {
        this.visible = false;
        this.$emit('update:note', this.note);
      },
      save() {
        this.hide();
      }
    }
  };
  </script>
  
  <style scoped>
  .popup {
    position: absolute;
    top: 40px;
    left: 50%;
    transform: translateX(-50%);
    width: 300px;
    background-color: #ffffff;
    border: 1px solid #d1d1d6;
    border-radius: 10px;
    padding: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    z-index: 1000;
  }
  
  textarea {
    width: 100%;
    height: 80px;
    resize: none;
    border: 1px solid #d1d1d6;
    border-radius: 5px;
    padding: 5px;
    font-family: inherit;
  }
  
  button {
    margin-top: 5px;
    padding: 5px 10px;
    background-color: #007aff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #0051a8;
  }
  </style>
  