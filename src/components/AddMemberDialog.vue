<template>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">Ajouter des amis ou des groupes</span>
        </v-card-title>
        <v-card-text>
          <v-checkbox label="Ami" v-model="isAddingFriend"></v-checkbox>
          <v-checkbox label="Groupe" v-model="isAddingGroup"></v-checkbox>
          <input type="text" v-model="searchQuery" @input="search" />
          <div v-for="result in searchResults" :key="result.id" @click="addUser(result)">
            {{ result.name }}
          </div>
          <div v-for="(user, index) in selectedUsers" :key="user.id">
            {{ user.name }}
            <v-btn small @click="selectedUsers.splice(index, 1)">Supprimer</v-btn>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="green darken-1" text @click="dialog = false">Fermer</v-btn>
          <v-btn color="green darken-1" text @click="add">Ajouter</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </template>
  
<script>
export default {
    data() {
        return {
            dialog: false,
            isAddingFriend: false,
            isAddingGroup: false,
            itemsToSearch: [], // liste des noms d'utilisateurs ou de groupes à chercher
            selectedItems: [],
            searchQuery: '',
            searchResults: [],
            selectedUsers: [],
        };
    },
    methods: {
        customFilter(item, queryText, itemText) {
            // déclenche la recherche à partir de 3 lettres
            if (queryText.length >= 3) {
                const text = itemText.toLowerCase();
                const query = queryText.toLowerCase();

                return text.indexOf(query) > -1;
            }

            return false;
        },
        add() {
            if (this.selectedItems.length <= 5) {
                // ajoutez votre logique ici pour ajouter des amis ou des groupes
            }
        },
        search() {
            if (this.searchQuery.length >= 3) {
                this.searchResults = this.users.filter(user =>
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
    },
    props: ['users', 'groups'],
};
</script>
