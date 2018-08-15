<template>
  <v-layout align-center justify-space-between row class="action-items">
    <div class="item">
      <v-icon title="Move element" class="item  actions move md-icon material-icons">
        drag_handle
      </v-icon>
    </div>
    <div class="item">
      <v-icon class="item actions delete md-icon material-icons" title="Delete element" @click="$emit('delete')">
        delete
      </v-icon>
    </div>
  </v-layout>
</template>

<script>
export default {
  name: 'icon-handler',
};
</script>

<style scoped>
  .action-items {
    color: #666666;
    z-index: 20;
  }

  .actions {
    cursor: pointer;
  }
</style>
