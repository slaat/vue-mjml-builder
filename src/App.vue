<template>
  <v-app dark>
    <router-view/>
  </v-app>
</template>

<script>
export default {
  name: 'App',
};
</script>

<style>
  .element:hover>i.actions {
    opacity: 1;
  }
</style>
