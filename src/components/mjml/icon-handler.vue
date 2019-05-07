<template>
  <v-layout align-center justify-space-between row class="action-items">
    <div class="item">
      <v-icon title="Move element" class="item actions move">
        drag_handle
      </v-icon>
      <v-icon title="Edit Element" class="item actions" @click="$emit('edit')">
        edit
      </v-icon>
    </div>
    <div class="item">
      <v-icon class="item actions delete" title="Delete element" @click="$emit('delete')">
        delete
      </v-icon>
    </div>
  </v-layout>
</template>

<script lang="ts">
  import {Component, Vue} from 'vue-property-decorator';

  @Component
  export default class IconHandler extends Vue {
  }
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
