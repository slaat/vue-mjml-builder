<template>
  <div v-if="data">
    <icon-handler @delete="$emit('delete')" @edit="editElement()"></icon-handler>
    <table width="100%" style="background-color: white;">
      <tr>
        <td align="center">
          <table align="center" border="0" cellpadding="0" cellspacing="0" role="presentation"
                 style="border-collapse:collapse;border-spacing:0px;">
            <tbody>
            <tr>
              <td :style="{ width: settings.width + 'px' }">
                <img
                  height="auto"
                  :src="settings.src"
                  style="border:0;display:block;outline:none;text-decoration:none;width:100%;"
                  width="300">
              </td>
            </tr>
            </tbody>
          </table>
        </td>
      </tr>
    </table>
    <v-dialog v-model="dialog" persistent max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">Element Settings</span>
        </v-card-title>
        <v-card-text>
          <v-container grid-list-md>
            <v-layout wrap>
              <v-flex xs6>
                <v-text-field
                  label="Width"
                  required
                  v-model="settings['width']"></v-text-field>
              </v-flex>
              <v-flex xs6>
                <v-text-field
                  label="Src (Image Url)"
                  required
                  v-model="settings['src']"></v-text-field>
              </v-flex>
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

<script>
import iconHandler from './icon-handler';

export default {
  name: 'mj-image',
  components: {
    iconHandler,
  },
  props: ['data', 'id'],
  data() {
    return {
      dialog: false,
      text: '',
      settings: {
        'padding-top': '0px',
        'padding-bottom': '0px',
        'padding-left': '0px',
        'padding-right': '0px',
        width: '',
        src: '',
      },
    };
  },
  methods: {
    editElement() {
      this.dialog = true;
    },
  },
  watch: {
    dialog() {
      this.$emit('updateSettings', this.settings);
    },
  },
  created() {
  },
};
</script>

<style scoped>

</style>
