<template>
    <div>
      <div class="header">
        <img src="../../assets/img/logo.png" class="circle-img" />
        <h1>SwitchOZ</h1>
      </div>
    
  
      <div class="filter-container row">
        <form @submit.prevent="filter">
          <label>
            <input type="checkbox" value="amis" v-model="internalFilters" /> Amis
          </label>
          <label>
            <input type="checkbox" value="groupes" v-model="internalFilters" /> Groupes
          </label>
          <label>
            <input type="checkbox" value="groupesAdmin" v-model="internalFilters" /> Groupes dont on est l’administrateur
          </label>
        </form>
  
        <div class="search-container">
          <input type="text" v-model="searchText" placeholder="Recherche.." />
  
          <div class="dropdown-content" v-show="searchText.length > 0 && filteredOptions.length > 0">
            <div v-for="option in filteredOptions" :key="option.id">
              <input type="checkbox" :id="option.id" :value="option.name" v-model="selectedUsers" />
              <label :for="option.id">{{ option.name }}</label>
            </div>
          </div>
        </div>
      </div>
      
      <div style="display: flex;
    justify-content: flex-end;">
      <!-- Bouton qui affiche la boîte de dialogue -->
      <button @click="openAddMemberDialog">Ajouter un membre</button>
    </div>
      <!-- Boîte de dialogue -->
      <AddMemberDialog
        :internalUsers="internalUsers"
        :groups="groups"
        :isOpen="isAddMemberDialogOpen"
        :result="myResult"
        @close="closeAddMemberDialog"
        @add-user="addMember"
        @remove="removeUser"
      ></AddMemberDialog>

      <div>
        <div v-for="user in filteredUsers" :key="user.id">
          <UserCard :user="user" @remove="removeUser" />
        </div>
        <div v-for="group in internalGroups" :key="group.id">
          <GroupCard :group="group" @remove="removeGroup" />
        </div>
      </div>
    </div>
  </template>

<script>
import UserCard from './UserCard.vue';
import GroupCard from './GroupCard.vue';
import AddMemberDialog from './AddMemberDialog.vue';

export default {
    components: {
        UserCard,
        GroupCard,
        AddMemberDialog
    },
    created() {
        console.log(this.$data.isAddMemberDialogOpen);  // affiche la liste des utilisateurs dans la console
        console.log(this.groups);  // affiche la liste des groupes dans la console
    },
    props: ['users', 'groups', 'filters', 'loggedInUser'],
    data() {
        return {
            myResult: null,
            internalUsers: this.users,
            internalGroups: this.groups,
            internalFilters: this.filters,
            isAddMemberDialogOpen: false,
            searchText: '',
            options: [],
            selectedUsers: [],
        };
    },
    computed: {
        filteredUsers: function () {
            if (this.internalUsers) {
                return this.internalUsers.filter(user => {
                    return (this.internalFilters.includes('amis') && user.isFriend) ||
                        (this.internalFilters.includes('groupes') && user.isGroup) ||
                        (this.internalFilters.includes('groupesAdmin') && user.isGroup && user.isAdmin);
                });
            } else {
                return [];
            }
        },
        filteredGroups: function () {
            let groups = [...this.internalGroups];
            if (this.internalFilters.includes('groupes')) {
                groups = groups.filter(group => group.members.some(member => member.id === this.loggedInUser.id));
            }
            if (this.internalFilters.includes('groupesAdmin')) {
                groups = groups.filter(group => group.admins.some(admin => admin.id === this.loggedInUser.id));
            }
            return groups;
        },
        filteredOptions() {
            if (!this.searchText) {
                return [];
            }
            let filter = this.searchText.toLowerCase();
            return this.internalUsers.filter(user => user.name.toLowerCase().includes(filter));
        },
        displayedUsers() {
          let usersToDisplay = this.internalUsers;
            
            if (this.searchText.trim() !== '') {
                let searchFilter = this.searchText.toLowerCase();
                usersToDisplay = usersToDisplay.filter(user => user.name.toLowerCase().includes(searchFilter));
            }

            if (this.selectedUsers.length > 0) {
                usersToDisplay = usersToDisplay.filter(user => this.selectedUsers.includes(user.name));
            }
            
            return usersToDisplay;
        }

    },
    methods: {
        openAddMemberDialog() {
            this.isAddMemberDialogOpen = true;
        },
        closeAddMemberDialog() {
            this.isAddMemberDialogOpen = false;
        },
        addUser(newUser) {
            if (!this.internalUsers.some(user => user.id === newUser.id)) {
                this.internalUsers.push(newUser);
            }
        },
        addMember(newUser) {
        if (!this.internalUsers.some(user => user.id === newUser.id)) {
            this.internalUsers.push(newUser);
        }
        console.log(this.internalUsers);
        },

        addGroup(newGroup) {
            if (!this.internalGroups.some(group => group.id === newGroup.id)) {
                this.internalGroups.push(newGroup);
            }
        },
        removeUser(user) {
            const index = this.internalUsers.findIndex(u => u.id === user.id);
            if (index !== -1) {
                if (confirm('Êtes-vous sûr de vouloir supprimer cet utilisateur ?')) {
                    this.internalUsers.splice(index, 1);
                }
            }
        },
        removeGroup(group) {
            const index = this.internalGroups.findIndex(g => g.id === group.id);
            if (index !== -1) {
                if (confirm('Êtes-vous sûr de vouloir supprimer ce groupe ?')) {
                    this.internalGroups.splice(index, 1);
                }
            }
        },
    }
}
</script>
