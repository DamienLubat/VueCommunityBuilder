<template>
    <div class="user-card">
        <img :src="user.picture" :alt="user.name" class="circle-img">
        <div class="user-info">
            <div>{{ user.name }}</div>
            <button v-if="canRemove" @click="$emit('remove')">Supprimer</button>
            <div v-if="user.isAdmin">Admin</div>
        </div>
    </div>
</template>

<script>
import UserCard from './UserCard.vue';

export default {
    props: ['user', 'canRemove']
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
</style>
