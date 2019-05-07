<template>
  <v-content>
    <v-container fluid class="fill-height">
      <v-layout row wrap>
        <v-flex lg1 xs4>
          <h2>Elements</h2>
          <draggable
            v-model="elements"
            :options="{forceFallback: true, group:{ name:'people',  pull:'clone', put:false }, sort: false}"
            @start="drag=true"
            @end="drag=false"
            :clone="insertComponent"
          >
            <div v-for="element in elements" :key="element.id" class="mjml-element">{{element.name}}</div>
          </draggable>
        </v-flex>
        <v-flex lg6 xs8>
          <h2>Builder</h2>
          <draggable
            v-model="activeElements"
            :options="{forceFallback: true, handle: 'i.actions.move', group:'people'}"
            @start="drag=true"
            @end="drag=false"
            class="dragArea editor-area elevation-2"
            data-empty-template="Drag Elements from the Elements Bar into this area"
            :class="{empty: !activeElements.length}"
          >
            <component
              v-for="(element, index) in activeElements"
              :key="element.id"
              :id="element.id"
              :is="element.type"
              :data="element.data"
              @delete="activeElements.splice(index, 1)"
              @updateSettings="updateElementSettings(index, ...arguments)"
              @updateContent="updateElementText(index, ...arguments)"/>
          </draggable>
        </v-flex>
        <v-flex lg5 xs12>
          <h2>Preview</h2>
          <v-tabs v-model="activeTab">
            <v-tab :key="1">Preview</v-tab>
            <v-tab :key="2">HTML</v-tab>
            <v-tab-item :key="1">
              <iframe :srcdoc="html" width="100%" style="background-color: white; min-height: 500px;" height="100%">
              </iframe>
            </v-tab-item>
            <v-tab-item :key="2" class="fill-height">
              <textarea style="height: 500px; width: 100%" disabled>{{html}}</textarea>
            </v-tab-item>
          </v-tabs>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script lang="ts">
  import {Component, Vue, Watch} from 'vue-property-decorator';
  import axios from 'axios';

  import draggable from 'vuedraggable';
  import MjButton from '@/components/mjml/mj-button.vue';
  import MjText from '@/components/mjml/mj-text.vue';
  import MjImage from '@/components/mjml/mj-image.vue';

  @Component({
    components: {
      MjButton,
      MjText,
      MjImage,
      draggable,
    },
  })
  export default class Generator extends Vue {
    private elements: any = [
      {name: 'text', id: '1', type: 'mj-text', data: '<p>Example Text</p>', settings: {}},
      {name: 'image', id: '3', type: 'mj-image', data: ' ', settings: {}},
      {name: 'button', id: '6', type: 'mj-button', data: 'Button', settings: {}},
    ];

    private activeElements: any = [];
    private drag: boolean = false;
    private html: string = '';
    private activeTab: number = 0;

    private insertComponent(obj) {
      const newEl = JSON.parse(JSON.stringify(obj));
      newEl.id = Date.now() + '_' + Math.random().toString(36).substr(2, 9);
      return newEl;
    }


    private deleteElement(index) {
      this.activeElements.splice(index, 1);
    }

    private updateElementSettings(index, settings) {
      this.activeElements[index].settings = settings;
      this.updateHtmlPreview();
    }

    private updateElementText(index, data) {
      this.activeElements[index].data = data;
      this.updateHtmlPreview();
    }

    private getMJML() {
      let mjml = '';
      this.activeElements.forEach((el) => {
        let settings = '';
        Object.keys(el.settings).forEach((key) => {
          settings += `${key}='${el.settings[key]}' `;
        });
        mjml += `<${el.type} ${settings}>${el.data}</${el.type}>`;
      });
      return `<mjml><mj-body><mj-section><mj-column>${mjml}</mj-column></mj-section></mj-body></mjml>`;
    }

    private updateHtmlPreview() {
      const mjml = this.getMJML();
      axios.post(
        'https://api.mjml.io/v1/render',
        {mjml},
        {
          headers: {
            // eslint-disable-next-line max-len
            'Authorization': 'Basic MTdiYTc3MDEtYzFhYS00OGJhLTg0MDctNDQzNTA1YWU1ZDQzOjk2MWQxMWUwLTlkZGMtNDdlZC05NWM2' +
              'LTgyNTk1MWU2MGQxNA==',
            'Content-Type': 'application/json',
          },
        },
      ).then((res) => {
        this.html = res.data.html;
      });
    }

    @Watch('activeElements')
    private arrayUpdated() {
      this.updateHtmlPreview();
    }
  }
</script>

<style scoped>

  @media (min-width: 600px) {
    .editor-menu {
      max-width: 100px;
    }
  }

  .editor-area {
    width: 600px;
    max-width: 600px;
    background-color: #424242;
  }

  .editor-area.empty {
    min-height: 500px;
    background: #424242;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  .editor-area.empty:after {
    content: attr(data-empty-template);
  }

  @media (max-width: 1366px) {
    .editor-area.empty {
      min-height: 400px;
    }
  }

  .editor-area .element.selected {
    box-shadow: 0 0 10px #ccc;
    z-index: 1;
  }

  .editor-area .element {
    position: relative;
    -webkit-transition: box-shadow, transform .1s ease-in-out;
    -moz-transition: box-shadow, transform .1s ease-in-out;
    -o-transition: box-shadow, transform .1s ease-in-out;
    transition: box-shadow, transform .1s ease-in-out;
  }

  .element > i.actions.delete {
    top: 0;
    right: 0;
  }

  .element > i.actions.move {
    top: 0;
    left: 0;
  }

  .element > i.actions {
    background-color: #d8d8d8;
    position: absolute;
    opacity: 0;
    margin: 1px;
    -webkit-transition: all .2s ease-in-out;
    -moz-transition: all .2s ease-in-out;
    -o-transition: all .2s ease-in-out;
    transition: all .2s ease-in-out;
  }

  .element {
    -webkit-transition: all .2s ease-in-out;
    -moz-transition: all .2s ease-in-out;
    -o-transition: all .2s ease-in-out;
    transition: all .2s ease-in-out;
  }

  .md-icon {
    font-size: 24px;
    line-height: 32px !important;
    height: 32px !important;
    color: #727272;
    border-radius: 50%;
    cursor: pointer;
    transition: background 280ms ease-out, color 280ms ease-out;
    width: 32px !important;
    text-align: center;
    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
  }

  .element:hover > i.actions {
    opacity: 1;
  }

  .columns {
    display: flex;
    flex: 1;
    overflow: auto;
    padding: 5px;
  }

  .main {
    flex: 1;
    order: 2;
    min-width: 600px;
    align-items: flex-start;
    display: flex;
    justify-content: center;
  }

  .sidebar-first {
    width: 66px;
    order: 1;
    z-index: 2;
  }

  .sidebar-second {
    min-width: 300px;
    order: 3;
    z-index: 2;
    padding: 15px;
  }

  .mjml-element {
    cursor: pointer;
  }
</style>
