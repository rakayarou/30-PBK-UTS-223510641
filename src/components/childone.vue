<template>
    <div class="child">
      <h3>Child One</h3>
      <button @click="sendData">Klik Tombol Ini</button>
    </div>
  </template>
  
  <script setup>
  import { defineEmits } from 'vue';
  
  const emit = defineEmits(['data-from-child']);
  
  const sendData = () => {
    emit('data-from-child', 'Data from child one');
  };
  </script>
  
  <style scoped>
  .child {
    border: 1px solid #f80355;
    padding: 10px;
    margin: 10px;
    text-align: center;
  }
  button {
    background-color: #f60e4f;
    color: white;
    border: none;
    padding: 10px 20px;
    cursor: pointer;
  }
  button:hover {
    background-color: #e95172;
  }
  </style>
