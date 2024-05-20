<template>
    <div class="child">
      <h3>Child Two</h3>
      <button @click="sendData">Klik Tombol Ini</button>
    </div>
  </template>
  
  <script setup>
  import { defineEmits } from 'vue';
  
  const emit = defineEmits(['data-from-child']);
  
  const sendData = () => {
    emit('data-from-child', 'Data from child two');
  };
  </script>
  
  <style scoped>
  .child {
    border: 1px solid #d21b45;
    padding: 10px;
    margin: 10px;
    text-align: center;
  }
  button {
    background-color: #ec114f;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
  }
  button:hover {
    background-color: #e4247e;
  }
  </style>
