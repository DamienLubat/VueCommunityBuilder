<template>
  <div class="user-card">
    <div class="circle-img-perso"></div>
    <div class="user-info">
      <div>{{ group.name }}</div>
      <button @click="remove">Supprimer</button>
      <div v-if="isAdmin">Admin</div>
    </div>
  </div>
</template>
  
  <script>
  export default {
    props: ['group', 'canRemove', 'isAdmin'],
    methods: {
      remove() {
            this.$emit('remove', this.group);
        },
    },
  }
  </script>
  
  <style scoped>
.user-card {
    display: flex;
    margin-bottom: 50px;
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
</style>