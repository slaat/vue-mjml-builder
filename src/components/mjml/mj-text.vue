<template>
  <div v-if="data">
    <icon-handler @delete="$emit('delete')" @edit="editElement()"></icon-handler>
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
            style="font-size: 13px; color: #000000;"
            :style="settings">
          <tinymce
            :id="id"
            v-model="text"
            :value="text"
            :inline="true"/>
        </td>
      </tr>
      </tbody>
    </table>
    <v-dialog v-model="dialog" persistent max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">Element Settings</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs12>
                <v-text-field
                  label="Padding Top"
                  required
                  v-model="settings['padding-top']"></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  label="Padding Right"
                  required
                  v-model="settings['padding-right']"></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  label="Padding Bottom"
                  required
                  v-model="settings['padding-bottom']"></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  label="Padding Left"
                  required
                  v-model="settings['padding-left']"></v-text-field>
              </v-flex>
            </v-layout>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" flat @click.native="dialog = false">Close</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script lang="ts">
  import {Component, Vue, Watch, Prop} from 'vue-property-decorator';
  import iconHandler from './icon-handler.vue';

  @Component({
    components: {
      iconHandler,
    },
  })
  export default class MjText extends Vue {
    @Prop({type: String, default: ''}) private data!: any;
    @Prop({type: String, default: ''}) private id!: any;

    private text: string = 'Example text';
    private dialog: boolean = false;
    private settings: any = {
      'padding-top': '10px',
      'padding-bottom': '10px',
      'padding-left': '25px',
      'padding-right': '25px',
    };

    private editElement() {
      this.dialog = true;
    }

    @Watch('dialog')
    private updatedDialog() {
      this.$emit('updateSettings', this.settings);
    }
    @Watch('text')
    private updatedText() {
      this.$emit('updateContent', this.text);
    }
  }
</script>

<style scoped>

</style>
