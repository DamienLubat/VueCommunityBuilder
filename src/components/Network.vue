<template>
    <div>
        <!-- Ajoutez ce bouton -->
        <button @click="openAddMemberDialog">Ajouter un membre</button>
        
        <!-- Ajoutez ce dialogue -->
        <AddMemberDialog :internalUsers="internalUsers" :groups="groups" :isOpen="isAddMemberDialogOpen" 
                 @close="closeAddMemberDialog" @add="addMembers"/>

        <form @submit.prevent="filter">
            <label>
                <input type="checkbox" value="amis" v-model="internalFilters"> Amis
            </label>
            <label>
                <input type="checkbox" value="groupes" v-model="internalFilters"> Groupes
            </label>
            <label>
                <input type="checkbox" value="groupesAdmin" v-model="internalFilters"> Groupes dont on est l’administrateur
            </label>
        </form>
        <div @submit.prevent="filter">
            <div v-for="user in internalUsers" :key="user.id">  <!-- Ici -->
                <UserCard :user="user" />
            </div>
            <div v-for="group in internalGroups" :key="group.id">  <!-- Et ici -->
                <GroupCard :group="group" />
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
            internalUsers: this.users,
            internalGroups: this.groups,
            internalFilters: this.filters,
            isAddMemberDialogOpen: false,  // Ajoutez ceci
        };
    },
    computed: {
        filteredUsers: function () {
        if (this.internalUsers) {   //Verifie si internalUsers est definie
            return this.internalUsers.filter(user => {
            return this.internalFilters.includes('amis') && user.isFriend ||
                    this.internalFilters.includes('groupes') && user.isGroup ||
                    this.internalFilters.includes('groupesAdmin') && user.isAdmin;
            });
        } else {
            return [];
        }
        },
        filteredGroups: function () {
        let groups = [...this.internalGroups];
        if (this.internalFilters.includes('groupes')) {
            groups = groups.filter(group => group.members.includes(this.loggedInUser.id));
        }
        if (this.internalFilters.includes('groupesAdmin')) {
            groups = groups.filter(group => group.admins.includes(this.loggedInUser.id));
        }
        return groups;
        }
    },
    methods: {
        openAddMemberDialog() {
            this.isAddMemberDialogOpen = true;
        },
        closeAddMemberDialog() {
            this.isAddMemberDialogOpen = false;
        },
        addMembers(newMembers) {
            newMembers.forEach(newMember => {
                if (!this.internalUsers.some(user => user.id === newMember.id)) {
                this.internalUsers.push(newMember);
                }
            });
            console.log(this.internalUsers);
        }
    }
}
</script>