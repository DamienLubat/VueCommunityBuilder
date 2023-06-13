<template>
    <div>
        <!-- Ajoutez ce bouton -->
        <button @click="isAddMemberDialogOpen = true">Ajouter un membre</button>

        <!-- Ajoutez ce dialogue -->
        <AddMemberDialog :users="users" :groups="groups" v-if="isAddMemberDialogOpen" @close="isAddMemberDialogOpen = false" />

        <form @submit.prevent="filter">
            <label>
                <input type="checkbox" value="amis" v-model="filters"> Amis
            </label>
            <label>
                <input type="checkbox" value="groupes" v-model="filters" > Groupes
            </label>
            <label>
                <input type="checkbox" value="groupesAdmin" v-model="filters" > Groupes dont on est l’administrateur
            </label>
        </form>
        <div v-for="user in filteredUsers" :key="user.id">
            <UserCard :user="user" :canRemove="canRemove(user)" @remove="removeUser(user)"/>
        </div>
        <div v-for="group in filteredGroups" :key="group.id">
            <GroupCard :group="group" :canRemove="canRemove(group)" @remove="removeGroup(group)"/>
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
    props: ['users', 'groups'],
    data() {
        return {
            filters: ['amis', 'groupes', 'groupesAdmin'],
            isAddMemberDialogOpen: false,
        };
    },
    computed: {
        filteredUsers: function() {
            if (this.users) {  // vérifie si users est définie
                return this.users.filter(user => {
                    return this.filters.includes('amis') && user.isFriend ||
                           this.filters.includes('groupes') && user.isGroup ||
                           this.filters.includes('groupesAdmin') && user.isAdmin;
                });
            } else {
                return [];  // retourne une liste vide si users est indéfinie
            }
        }, 
        filteredGroups: function() {
            let groups = [...this.groups];
            if (this.filters.groups) {
                groups = groups.filter(group => group.members.includes(this.loggedInUser.id));
            }
            if (this.filters.adminGroups) {
                groups = groups.filter(group => group.admins.includes(this.loggedInUser.id));
            }
            return groups;
        }
    },
    methods: {
        canRemove(item) {
            // implémentez la logique ici
        },
        removeUser(user) {
            // implémentez la logique ici
        },
        removeGroup(group) {
            // implémentez la logique ici
        },
    }
}
</script>
