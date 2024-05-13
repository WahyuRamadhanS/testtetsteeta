<template>
    <div class="container">
      <select v-model="selectedUser" @change="fetchPosts" class="select">
        <option value="">Select User</option>
        <option v-for="user in users" :value="user.id" :key="user.id">{{ user.name }}</option>
      </select>
      
      <div v-if="selectedUser" class="post-container">
        <h2 class="title">Posts by {{ selectedUserName }}</h2>
        <ul class="post-list">
          <li v-for="post in posts" :key="post.id" class="post-item">
            <h3 class="post-title">{{ post.title }}</h3>
            <p class="post-body">{{ post.body }}</p>
          </li>
        </ul>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedUser: '',
        selectedUserName: '',
        users: [],
        posts: []
      };
    },
    methods: {
      async fetchUsers() {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users');
          const data = await response.json();
          this.users = data;
        } catch (error) {
          console.error(error);
        }
      },
      async fetchPosts() {
        if (this.selectedUser) {
          try {
            const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
            const data = await response.json();
            this.posts = data;
            this.selectedUserName = this.users.find(user => user.id === parseInt(this.selectedUser)).name;
          } catch (error) {
            console.error(error);
          }
        } else {
          this.posts = [];
          this.selectedUserName = '';
        }
      }
    },
    mounted() {
      this.fetchUsers();
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 1400px;
    margin: 0 auto;
    padding: 20px;
    background-color: #fefefe; /* Warna cream */
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }
  
  .select {
    width: 100%;
    padding: 10px;
    margin-bottom: 20px;
    border: 1px solid #ccc;
    border-radius: 4px;
    font-size: 16px;
  }
  
  .post-container {
    margin-top: 20px;
  }
  
  .title {
    font-size: 24px;
    margin-bottom: 10px;
    color: #333; /* Warna hitam */
  }
  
  .post-list {
    list-style: none;
    padding: 0;
  }
  
  .post-item {
    margin-bottom: 20px;
  }
  
  .post-title {
    font-size: 18px;
    margin-bottom: 5px;
    color: #555; /* Warna abu-abu */
  }
  
  .post-body {
    color: #666; /* Warna abu-abu sedikit lebih terang */
  }
  </style>
  