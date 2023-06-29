<template>
    <div>
      <div class="filter-container row">
        <div class="header">
            <img src="../../assets/img/logo.png" class="circle-img" />
            <h1>SwitchOZ</h1>
        </div>

        <div style="display: flex; justify-content: flex-end;">
            <svg @click="toggleMasterFiltre" xmlns="http://www.w3.org/2000/svg" width="29" height="100" viewBox="0 0 24 24" fill="none" stroke="rgba(225, 237, 222)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                <line x1="5" y1="5" x2="20" y2="5"></line>
                <line x1="5" y1="10" x2="20" y2="10"></line>
                <line x1="5" y1="15" x2="20" y2="15"></line>
            </svg>
        </div>
      </div>
      <div v-show="isMasterFilterActive">
        <div style="display: flex; justify-content: flex-end;">
                <form @submit.prevent="filter" v-show="isFilterActive">
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
                <svg @click="toggleFiltre" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="rgba(225, 237, 222)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <polygon points="22 3 2 3 10 12.46 10 19 14 21 14 12.46 22 3"></polygon>
                </svg>
        </div>
            <div style="display: flex; justify-content: flex-end;">
                <svg @click="toggleSearch" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="rgba(225, 237, 222)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <circle cx="11" cy="11" r="6"></circle>
                    <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
                </svg>
                <div class="dropdown-content" v-show="isSearchActive">
                    <input type="search" v-model="searchText" placeholder="Recherche.."/>
                    <div class="dropdown-content" v-show="searchText.length > 0 && filteredOptions.length > 0">
                        <div v-for="option in filteredOptions" :key="option.id">
                            <input type="checkbox" :id="option.id" :value="option.name" v-model="selectedUsers" />
                            <label :for="option.id">{{ option.name }}</label>
                        </div>
                    </div>
                </div>
            </div>

            <div style="display: flex; justify-content: flex-end;">
                <!-- Bouton qui affiche la boîte de dialogue -->
                <svg @click="openAddMemberDialog" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 25 25" fill="none" stroke="rgba(225, 237, 222)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-user-plus">
                    <path d="M8 9a4 4 0 0 1 8 0 4 4 0 0 1-8 0z"></path>
                    <path d="M4 20v-1a4 4 0 0 1 4-4h8a4 4 0 0 1 4 4v1"></path>
                    <line x1="21" y1="8" x2="21" y2="14"></line>
                    <line x1="24" y1="11" x2="18" y2="11"></line>
                </svg>
            </div>
            <!-- Boîtex de dialogue -->
            <AddMemberDialog
                :internalUsers="internalUsers"
                :groups="groups"
                :isOpen="isAddMemberDialogOpen"
                :result="myResult"
                @close="closeAddMemberDialog"
                @add-user="addMember"
                @remove="removeUser"
            ></AddMemberDialog>
        </div>
      <div>
        <h3>Amis</h3>
        <div v-for="user in filteredUsers" :key="user.id">
          <UserCard :user="user" @remove="removeUser" />
        </div>

        <h3>Groupes</h3>

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
            isSearchActive: false,
            isFilterActive: false,
            isMasterFilterActive: false,
        };
    },
    computed: {
        filteredUsers: function () {
            let usersToProcess = this.internalUsers;

            if (this.internalUsers) {
                usersToProcess = this.internalUsers.filter(user => {
                    return (this.internalFilters.includes('amis') && user.isFriend) ||
                        (this.internalFilters.includes('groupes') && user.isGroup) ||
                        (this.internalFilters.includes('groupesAdmin') && user.isGroup && user.isAdmin);
                });
            } else {
                return [];
            }

            if (this.searchText.trim() !== '') {
                let searchFilter = this.searchText.toLowerCase();

                if (this.selectedUsers.length > 0) {
                    usersToProcess = usersToProcess.filter(user => this.selectedUsers.includes(user.name));
                }
            }

            return usersToProcess;
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
            return [...this.internalUsers.filter(user => user.name.toLowerCase().includes(filter)), 
                    ...this.internalGroups.filter(group => group.name.toLowerCase().includes(filter))];
        },
        displayedUsers() {
            let usersToDisplay = this.internalUsers;
            
            if (this.searchText.trim() !== '') {
                let searchFilter = this.searchText.toLowerCase();

                if (this.selectedUsers.length > 0) {
                    usersToDisplay = usersToDisplay.filter(user => this.selectedUsers.includes(user.name));
                }
                //Selection automatique des utilisateurs
                /* else {
                    usersToDisplay = usersToDisplay.filter(user => user.name.toLowerCase().includes(searchFilter));
                }*/
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
        toggleSearch: function() {
            this.isSearchActive = !this.isSearchActive;
        },
        toggleFiltre: function() {
            this.isFilterActive = !this.isFilterActive;
        },
        toggleMasterFiltre: function() {
            this.isMasterFilterActive = !this.isMasterFilterActive;
        },
    }
}
</script>
