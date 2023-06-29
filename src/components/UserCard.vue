<template>
    <div class="user-card">
        <img :src="user.picture" :alt="user.name" class="circle-img-perso">
        <div class="user-info">
            <div>{{ user.name }}</div>
            <button @click="remove">Supprimer</button>
            <div v-if="user.isAdmin">
                <img :src="adminIcon" alt="Admin Icon" class="small-circle-img">
            </div>
        </div>
    </div>
</template>

<script>
import UserCard from './UserCard.vue';
import adminIcon from '@/../assets/img/admin.png';


export default {
    data() {
        return {
            adminIcon,
        };
    },
    props: ['user', 'canRemove'],
    methods: {
        remove() {
            this.$emit('remove', this.user);
        },
    },
}
</script>

<style scoped>
.user-card {
    display: flex;
}
.circle-img {
    width: 200px;
    height: 200px;
    object-fit: cover;
    border-radius: 50%; 
} 

.user-info {
    
    margin: auto;
}

.user-card {
    margin-bottom: 50px;
}

.small-circle-img {
    width: 30px;
    height: 30px;
    object-fit: cover;
    border-radius: 50%; 
}
</style>
