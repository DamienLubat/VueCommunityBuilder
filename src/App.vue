<script>
import { RouterLink, RouterView } from 'vue-router'
import UserCard from './components/UserCard.vue'
import Search from './components/Search.vue'
import Network from './components/Network.vue'
import GroupCard from './components/GroupCard.vue'


export default {
  name: 'App',
  components: {
    Network
  },
  data() {
    return {
      users: [
        { id: 1, name: 'Albert Einstein', picture: './assets/img/albert-einstein.png', isAdmin: false },
        { id: 2, name: 'Roberto Firmino', picture: './assets/img/roberto-firmino.png', isAdmin: false },
        { id: 2, name: 'James Milner', picture: './assets/img/james-milner.jpeg', isAdmin: true },
      ],
      groups: [ 
        { id: 1, name: 'scientifique'},
        { id: 2, name: 'footballeur'},
       ],
      loggedInUser: {
        name: "Damien Lubat",
        username: "Luwer12",
        isAdmin: true
        // autres détails ici
      }
    }
  }
}
</script>

<template>
  <div id="app">
    <Network :users="users" :groups="groups" />
  </div>
</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

</style>
