<template>
  <!-- Creation of the dialog box that appears when isOpen is true -->
  <div class="dialog" v-if="isOpen">
    <!-- Creation of a card that will contain all the dialog elements -->
    <div class="card">
      <!-- Card title -->
      <h3>Ajouter des amis ou des groupes</h3>
      <!-- Section to choose between friends or groups -->
      <div>
        <!-- Checkbox to choose to add a friend -->
        <input type="checkbox" id="friend" v-model="isAddingFriend" />
        <label for="friend" id="checkout">Amis</label>
        <!-- Checkbox to choose to add a group -->
        <input type="checkbox" id="group" v-model="isAddingGroup" />
        <label for="group" id="checkout">Groupes</label>
      </div>
      <!-- List of filtered friends -->
      <div v-for="user in filteredUsers" :key="user.id">
        {{ user.name }} est déjà utilisée
      </div>
      <!-- List of filtered groups -->
      <div v-for="group in filteredGroups" :key="group.id">
        {{ group.name }} est déjà utilisée
      </div>
      <!-- Buttons to close the dialog box and add users to the list -->
        <!-- Text field to search for users or groups -->
        <input v-model="searchQuery" type="text" placeholder="Search..." />
        <!-- List of selected users with an option to remove them -->
        <div v-for="(user, index) in selectedUsers" :key="user.id">
          {{ user.name }}
          <button @click="removeUser(user, index)">Supprimer</button>
        </div>
        <button @click="add">Ajouter</button>

          <button @click="$emit('close')" style="float: right;">Fermer</button>

    </div>
  </div>
</template>
  
<script>
export default {
  data() {
    return {
      // Data initialization
      result: null,
      searchQuery: '',         // search query
      isAddingFriend: false,   // add a friend
      isAddingGroup: false,    // add a group
      searchResults: [],       // search results
      selectedUsers: [],       // selected users
      selectedGroups: [],      // selected groups
    };
  },
  computed: {
    // Method to filter users based on the search query
    filteredUsers() {
      if (this.searchQuery.length < 3) {
        return [];
      }
      return this.internalUsers.filter(user =>
        user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
    },
    // Method to filter groups based on the search query
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
    // Method to perform the search
    search() {
      if (this.searchQuery.length >= 3) {
        this.searchResults = this.users.filter(user =>
          user.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      } else {
        this.searchResults = [];
      }
    },
    // Method to add a user to the list of selected users
    addUser(user) {
      if (this.selectedUsers.length < 5 && !this.selectedUsers.includes(user)) {
        this.selectedUsers.push(user);
      }
    },
    // Method to add a group to the list of selected groups
    addGroup(group) {
      if (this.selectedGroups.length < 5 && !this.selectedGroups.includes(group)) {
        this.selectedGroups.push(group);
      }
    },
    // Method to add all searched users to the list of selected users
    add() {
      if (this.isAddingFriend) {
        // Create a new user with the entered text
        let newUser = {
          id: this.internalUsers.length + 1,  // Generate a unique id based on the length of the user list
          name: this.searchQuery,  // Use the entered text as the name
          picture: '',  // Use a default image or allow the user to upload one
          isAdmin: false,  // By default, the new user is not an admin
          isGroup: false,  // By default, the new user is not a group
          isFriend: true,  // By default, the new user is not a friend
        };

        // Add the new user to the list of selected users
        if (this.selectedUsers.length < 5 && !this.selectedUsers.includes(newUser)) {
          this.selectedUsers.push(newUser);
          if (!this.internalUsers.some(user => user.id === newUser.id)) {
            this.internalUsers.push(newUser);
        }
        }
        if (this.selectedUsers.length >= 5) {
          console.log("Vous ne pouvez pas ajouter plus de 5 éléments à la liste");
        }
        
      }
      if (this.isAddingGroup) {
         // Create a new group with the entered text
          let newGroup = {
            id: this.groups.length + 1,  // Generate a unique id based on the length of the group list
            name: this.searchQuery,  // Use the entered text as the name
            members: [],  // Initialize the list of group members to empty
            admins: [],  // Initialize the list of group admins to empty
          };

          // Add the new group to the list of selected groups
          this.groups.push(newGroup);

          this.$emit('add-group', newGroup);
      }
    },
    // Method to close the dialog box
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
  props: ['internalUsers', 'groups', 'isOpen'], // Receiving properties from the parent
  watch: {
    // Watching the isOpen property for changes
    isOpen(newVal) {
      this.dialog = newVal;
    },
  },
};
</script>
