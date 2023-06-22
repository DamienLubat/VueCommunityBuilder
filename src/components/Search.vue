<template>
  <div class="search">
    <input type="text" v-model="searchQuery" placeholder="Search" @input="search" />
    <div v-for="result in searchResults" :key="result.id">
      <!-- Affichez les résultats de recherche ici -->
      <div>{{ result.name }}</div>
    </div>
  </div>
</template>
  
<script>
  import albertEinstein from '@/assets/img/albert-einstein.png'

  export default {
    data() {
      return {
        searchQuery: '',
        searchResults: [],
        users: [
          { id: 1, name: 'Albert Einstein', picture: './assets/img/albert-einstein.png', isAdmin: false, isGroup: false, isFriend: false },
          { id: 2, name: 'Roberto Firmino', picture: './assets/img/roberto-firmino.png', isAdmin: false, isGroup: false, isFriend: true },
          { id: 3, name: 'James Milner', picture: './assets/img/james-milner.jpeg', isAdmin: true, isGroup: true, isFriend: true },
        ],
      }
    },
    methods: {
      search() {
        if (this.searchQuery.length >= 3) {
          this.searchResults = this.users.filter(user =>
            user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
          );
        } else {
          this.searchResults = [];
        }
      }
    }
  }
</script>

  