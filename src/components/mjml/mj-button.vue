<template>
  <div @click="editorOpen = true">
    Button {{ data }} {{ editorOpen }}
    <attribute-editor :open="editorOpen" />
  </div>
</template>

<script>
import AttributeEditor from './attribute-editor';

export default {
  name: 'mj-button',
  components: {
    AttributeEditor,
  },
  props: ['data'],
  data() {
    return {
      editorOpen: false,
    };
  },
};
</script>

<style scoped>

</style>
