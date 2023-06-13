<script>
import { RouterLink, RouterView } from 'vue-router'
import UserCard from './components/UserCard.vue'
import Search from './components/Search.vue'
import Network from './components/Network.vue'
import GroupCard from './components/GroupCard.vue'
import AddMemberDialog from './components/AddMemberDialog.vue';

export default {
  name: 'App',
  components: {
    Network,
    AddMemberDialog
  },
  /*created() {
    console.log(this.users);  // affiche la liste des utilisateurs dans la console
    console.log(this.groups);  // affiche la liste des groupes dans la console
  },*/
  data() {
    return {
      users: [
          { id: 1, name: 'Albert Einstein', picture: './assets/img/albert-einstein.png', isAdmin: false, isGroup: true, isFriend: false },
          { id: 2, name: 'Roberto Firmino', picture: './assets/img/roberto-firmino.png', isAdmin: false, isGroup: true, isFriend: true },
          { id: 3, name: 'James Milner', picture: './assets/img/james-milner.jpeg', isAdmin: true, isGroup: true, isFriend: true },
        ],
      groups: [ 
        { id: 1, name: 'scientifique'},
        { id: 2, name: 'footballeur'},
       ],
      filters: ['amis', 'groupes', 'groupesAdmin'], // ajoutez cette ligne
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
    <Network :users="users" :groups="groups" s:filters="filters" :loggedInUser="loggedInUser" />
    <AddMemberDialog />
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
