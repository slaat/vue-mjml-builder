<template>
  <div v-if="data">
    <icon-handler @delete="$emit('delete')" @edit="editElement()"></icon-handler>
    <table
      class="reset-this-parent"
      align="center"
      border="0"
      cellpadding="0"
      cellspacing="0"
      role="presentation"
      style="background-color: white;border-collapse:separate;line-height:100%;">
      <tr>
        <td
          align="center"
          bgcolor="#414141"
          role="presentation"
          style="border:none;border-radius:3px;color:#ffffff;cursor:auto;padding:10px 25px;"
          valign="middle">
          <p
            style="background:#414141;color:#ffffff;font-family:Ubuntu, Helvetica, Arial, sans-serif;font-size:13px;
            font-weight:normal;line-height:120%;Margin:0;text-decoration:none;text-transform:none;">
            Don't click me!
          </p>
        </td>
      </tr>
    </table>
    <!--<table width="600"-->
           <!--class="main"-->
           <!--cellspacing="0"-->
           <!--cellpadding="0"-->
           <!--border="0"-->
           <!--style="display: table; background-color: white;"-->
           <!--align="center"-->
           <!--data-type="text-block">-->
      <!--<tbody>-->
      <!--<tr>-->
        <!--<td class="block-text" data-block-id="background" align="left"-->
            <!--style="font-size: 13px; color: #000000;"-->
            <!--:style="settings">-->
          <!--<tinymce-->
            <!--:id="id"-->
            <!--v-model="text"-->
            <!--:value="text"-->
            <!--:inline="true"/>-->
        <!--</td>-->
      <!--</tr>-->
      <!--</tbody>-->
    <!--</table>-->
    <v-dialog v-model="dialog" persistent max-width="500px">
      <v-card>
        <v-card-title>
          <span class="headline">User Profile</span>
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
      settings: {
        'padding-top': '0px',
        'padding-bottom': '0px',
        'padding-left': '0px',
        'padding-right': '0px',
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
};
</script>

<style scoped>

</style>
