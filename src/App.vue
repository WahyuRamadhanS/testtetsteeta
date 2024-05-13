<script setup>
import { ref } from "vue";
import Todos from './components/todos.vue'; // nama komponen yang sudah diperbaiki
import Posts from './components/post.vue'; // nama komponen yang sudah diperbaiki

const SelectedMenu = ref("");
</script>

<template>
    <header>
        <nav>    
        <button @click="SelectedMenu = 'todos'">todos</button>
        <button @click="SelectedMenu = 'posts'">post</button>

        <div v-if="SelectedMenu === 'todos'">   
            <Todos /> 
        </div>
        <div v-if="SelectedMenu === 'posts'"> 
            <Posts />
        </div>
  </nav>
    </header>
  
</template>

  <style scoped>
  .header {
    background-color: #4CAF50; /* Warna latar belakang hijau */
    padding: 20px;
    color: white; /* Warna teks putih */
    display: flex;
    justify-content: space-between; /* Teks dan tombol akan berada di sisi berlawanan */
    align-items: center; /* Teks dan tombol akan berada di tengah secara vertikal */
  }
  
  .toggle-button {
    background-color: #008CBA; /* Warna latar belakang biru */
    color: white; /* Warna teks putih */
    border: none;
    padding: 10px 20px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 4px; /* Tambahkan sedikit sudut pada tombol */
  }
  
  .toggle-button:hover {
    background-color: #005f7b; /* Saat kursor mengarah, warna biru yang lebih gelap */
  }
  
  .header-title {
    margin: 0; /* Hapus margin default */
  }
  </style>
  