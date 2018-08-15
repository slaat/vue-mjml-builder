<template>
  <div v-if="data">
    <icon-handler @delete="$emit('delete')"></icon-handler>
    <table width="600"
           class="main"
           cellspacing="0"
           cellpadding="0"
           border="0"
           style="display: table; background-color: white;"
           align="center"
           data-type="text-block">
      <tbody>
      <tr>
        <td class="block-text" data-block-id="background" align="left"
            style="font-size: 13px; color: #000000; line-height: 22px;">
          <tinymce
            :id="id"
            v-model="text"
            :value="text"
            :inline="true"/>
        </td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import iconHandler from './icon-handler';

export default {
  name: 'mj-text',
  components: {
    iconHandler,
  },
  props: ['data', 'id'],
  data() {
    return {
      text: 'Example text',
    };
  },
};
</script>

<style scoped>

</style>
