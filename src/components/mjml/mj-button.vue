<template>
  <div v-if="data">
    <icon-handler @delete="$emit('delete')" @edit="editElement()"></icon-handler>
    <table width="100%" style="background-color: white;">
      <tr>
        <td align="center">
          <table
            role="presentation"
            cellpadding="0"
            cellspacing="0"
            style="border-collapse:separate;"
            align="center"
            border="0">
            <tr>
              <td style="border:none;border-radius:3px;color:#ffffff;cursor:auto;padding:10px 25px;"
                  align="center"
                  valign="middle"
                  :bgcolor="settings['background-color']">
                <p
                  :style="settings"
                  style="text-decoration:none;font-family:Ubuntu, Helvetica, Arial, sans-serif;
                  font-size:13px;font-weight:normal;line-height:120%;text-transform:none;margin:0px;">
                  {{text}}
                </p>
              </td>
            </tr>
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
              <v-flex xs12>
                <v-text-field
                  label="Text"
                  required
                  v-model="text"></v-text-field>
              </v-flex>
              <v-flex xs12>
                <v-text-field
                  label="Href"
                  required
                  v-model="settings['href']"></v-text-field>
              </v-flex>
              <v-flex xs6>
                <v-text-field
                  label="Background Color"
                  required
                  v-model="settings['background-color']"></v-text-field>
              </v-flex>
              <v-flex xs6>
                <v-text-field
                  label="Text Color"
                  required
                  v-model="settings['color']"></v-text-field>
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
  name: 'mj-button',
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
        'background-color': '#414141',
        href: '',
        color: '#ffffff',
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
    text() {
      this.$emit('updateContent', this.text);
    },
  },
  created() {
    this.text = this.data;
  },
};
</script>

<style scoped>

</style>
