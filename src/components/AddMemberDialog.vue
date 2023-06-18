<template>
  <div class="dialog" v-if="isOpen">
    <div class="card">
      <h2>Ajouter des amis ou des groupes</h2>
      <div>
        <input type="checkbox" id="ami" v-model="isAddingFriend" />
        <label for="ami">Ami</label>
        <input type="checkbox" id="groupe" v-model="isAddingGroup" />
        <label for="groupe">Groupe</label>
      </div>
      <input type="text" v-model="searchQuery" @input="search" />
      <div v-for="result in searchResults" :key="result.id" @click="addUser(result)">
        {{ result.name }}
      </div>
      <div v-for="(user, index) in selectedUsers" :key="user.id">
        {{ user.name }}
        <button @click="selectedUsers.splice(index, 1)">Supprimer</button>
      </div>
      <div>
        <button @click="$emit('close')">Fermer</button>
        <button @click="add">Ajouter</button>
      </div>
    </div>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      isAddingFriend: false,
      isAddingGroup: false,
      searchQuery: '',
      searchResults: [],
      selectedUsers: [],
    };
  },
  methods: {
    search() {
      // déclenche la recherche à partir de 3 lettres
      if (this.searchQuery.length >= 3) {
        this.searchResults = this.$props.users.filter(user =>
          user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.searchResults = [];
      }
    },
    addUser(user) {
      if (this.selectedUsers.length < 5 && !this.selectedUsers.includes(user)) {
        this.selectedUsers.push(user);
      }
    },
    add(user) {
      // Vous devez vérifier si la longueur de selectedUsers n'est pas déjà à 5.
      // S'il y a de la place, ajoutez tous les utilisateurs recherchés.
      if (this.selectedUsers.length < 5) {
        this.searchResults.forEach(user => {
          if (!this.selectedUsers.includes(user)) {
            this.selectedUsers.push(user);
          }
        });
      } else {
        console.log("Vous ne pouvez pas ajouter plus de 5 éléments à la liste");
      }
      this.$emit('add', this.selectedUsers);
    },
    close() {
      this.$emit('close');
    },
  },
  props: ['users', 'groups', 'isOpen'],
  watch: {
    isOpen(newVal) {
      this.dialog = newVal;
    },
  },
};
</script>
