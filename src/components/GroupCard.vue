<template>
    <div>
      <div>{{ group.name }}</div>
      <button @click="remove">Supprimer</button>
      <div v-if="isAdmin">Admin</div>
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
  