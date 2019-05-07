<template>
  <div>
  </div>
</template>

<script lang="ts">
  import {Component, Vue, Prop} from 'vue-property-decorator';

  @Component
  export default class AttributeEditor extends Vue {
    @Prop({type: String, default: ''}) private data!: any;
    @Prop({type: String, default: ''}) private open!: any;
  }
</script>

<style scoped>

</style>
