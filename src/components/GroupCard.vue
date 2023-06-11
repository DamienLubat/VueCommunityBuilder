<template>
    <div>
      <div>{{ group.name }}</div>
      <button v-if="canRemove" @click="$emit('remove')">Supprimer</button>
      <div v-if="isAdmin">Admin</div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['group', 'canRemove', 'isAdmin']
  }
  </script>
  