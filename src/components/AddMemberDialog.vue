<template>
  <!-- Création de la boite de dialogue qui apparaît quand isOpen est à true -->
  <div class="dialog" v-if="isOpen">
    <!-- Création d'une carte qui contiendra tous les éléments du dialogue -->
    <div class="card">
      <!-- Titre de la carte -->
      <h2>Ajouter des amis ou des groupes</h2>
      <!-- Section pour choisir entre amis ou groupes -->
      <div>
        <!-- Checkbox pour choisir d'ajouter un ami -->
        <input type="checkbox" id="ami" v-model="isAddingFriend" />
        <label for="ami">Ami</label>
        <!-- Checkbox pour choisir d'ajouter un groupe -->
        <input type="checkbox" id="groupe" v-model="isAddingGroup" />
        <label for="groupe">Groupe</label>
      </div>
      <!-- Liste des amis filtrés -->
      <div v-for="user in filteredUsers" :key="user.id">
        {{ user.name }} est déjà utilisée
      </div>
      <!-- Liste des groupes filtrés -->
      <div v-for="group in filteredGroups" :key="group.id">
        {{ group.name }} est déjà utilisée
      </div>
      <!-- Boutons pour fermer la boite de dialogue et ajouter des utilisateurs à la liste -->
        <!-- Zone de texte pour rechercher des utilisateurs ou des groupes -->
        <input v-model="searchQuery" type="text" placeholder="Search..." />
        <!-- Liste des utilisateurs sélectionnés avec une option pour les supprimer -->
        <div v-for="(user, index) in selectedUsers" :key="user.id">
          {{ user.name }}
          <button @click="removeUser(user, index)">Supprimer</button>
        </div>
        <button @click="$emit('close')">Fermer</button>
        <button @click="add">Ajouter</button>
    </div>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      // Initialisation des données
      result: null,
      searchQuery: '',         // requête de recherche
      isAddingFriend: false,   // ajouter un ami
      isAddingGroup: false,    // ajouter un groupe
      searchResults: [],       // résultats de la recherche
      selectedUsers: [],       // utilisateurs sélectionnés
      selectedGroups: [],      // groupes sélectionnés
    };
  },
  computed: {
    // Méthode pour filtrer les utilisateurs en fonction de la requête de recherche
    filteredUsers() {
      if (this.searchQuery.length < 3) {
        return [];
      }
      return this.internalUsers.filter(user =>
        user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    // Méthode pour filtrer les groupes en fonction de la requête de recherche
    filteredGroups() {
      if (this.searchQuery.length < 3) {
        return [];
      }
      return this.groups.filter(group =>
        group.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    }
  },
  methods: {
    // Méthode pour effectuer la recherche
    search() {
      if (this.searchQuery.length >= 3) {
        this.searchResults = this.users.filter(user =>
          user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.searchResults = [];
      }
    },
    // Méthode pour ajouter un utilisateur à la liste des utilisateurs sélectionnés
    addUser(user) {
      if (this.selectedUsers.length < 5 && !this.selectedUsers.includes(user)) {
        this.selectedUsers.push(user);
      }
    },
    // Méthode pour ajouter un groupe à la liste des groupes sélectionnés
    addGroup(group) {
      if (this.selectedGroups.length < 5 && !this.selectedGroups.includes(group)) {
        this.selectedGroups.push(group);
      }
    },
    // Méthode pour ajouter tous les utilisateurs recherchés à la liste des utilisateurs sélectionnés
    add() {
      if (this.isAddingFriend) {
        // Créer un nouvel utilisateur avec le texte saisi
        let newUser = {
          id: this.internalUsers.length + 1,  // Générer une id unique basée sur la longueur de la liste des utilisateurs
          name: this.searchQuery,  // Utiliser le texte saisi comme nom
          picture: '',  // Utiliser une image par défaut ou permettre à l'utilisateur de la télécharger
          isAdmin: false,  // Par défaut, le nouvel utilisateur n'est pas un admin
          isGroup: false,  // Par défaut, le nouvel utilisateur n'est pas un groupe
          isFriend: true,  // Par défaut, le nouvel utilisateur n'est pas un ami
        };

        // Ajouter le nouvel utilisateur à la liste des utilisateurs sélectionnés
        if (this.selectedUsers.length < 5 && !this.selectedUsers.includes(newUser)) {
          this.selectedUsers.push(newUser);
        }
        if (this.selectedUsers.length >= 5) {
          console.log("Vous ne pouvez pas ajouter plus de 5 éléments à la liste");
        }
        this.$emit('add', this.selectedUsers);
      }
      if (this.isAddingGroup) {
         // Créer un nouveau groupe avec le texte saisi
          let newGroup = {
            id: this.groups.length + 1,  // Générer une id unique basée sur la longueur de la liste des groupes
            name: this.searchQuery,  // Utiliser le texte saisi comme nom
            members: [],  // Initialiser la liste des membres du groupe à vide
            admins: [],  // Initialiser la liste des admins du groupe à vide
          };

          // Ajouter le nouveau groupe à la liste des groupes sélectionnés
          this.groups.push(newGroup);

          this.$emit('add-group', newGroup);
      }
    },
    // Méthode pour fermer la boite de dialogue
    close() {
      this.$emit('close');
    },
    removeUser(user, index) {
    if (user) {
      this.selectedUsers.splice(index, 1);
      this.$emit('remove', user);
    }
  }
  },
  props: ['internalUsers', 'groups', 'isOpen'], // Réception des propriétés du parent
  watch: {
    // Surveillance de la propriété isOpen pour des changements
    isOpen(newVal) {
      this.dialog = newVal;
    },
  },
};
</script>
