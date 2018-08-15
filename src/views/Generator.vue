<template>
  <v-content>
    <v-container fluid>
      <v-layout row wrap>
        <v-flex xs6>
          <draggable
            v-model="myArray"
            :options="{forceFallback: true, group:{ name:'people',  pull:'clone', put:false }, sort: false}"
            @start="drag=true"
            @end="drag=false"
            :clone="insertComponent"
          >
            <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
          </draggable>
        </v-flex>
        <v-flex xs6>
          <draggable
            v-model="myArray2"
            :options="{forceFallback: true, handle: 'i.actions.move', group:'people'}"
            @start="drag=true"
            @end="drag=false"
            class="dragArea editor-area elevation-2"
            data-empty-template="Ziehe Elemente hier hinein"
            :class="{empty: !myArray2.length}"
          >
            <component
              v-for="(element, index) in myArray2"
              :key="element.id"
              :id="element.id"
              :is="element.type"
              :data="element.data"
              @delete="myArray2.splice(index, 1)"/>
          </draggable>
        </v-flex>
      </v-layout>
    </v-container>
  </v-content>
</template>

<script>
import draggable from 'vuedraggable';
import MjButton from '../components/mjml/mj-button';
import MjColumn from '../components/mjml/mj-column';
import MjText from '../components/mjml/mj-text';
import MjHeader from '../components/mjml/mj-header';
import MjSection from '../components/mjml/mj-section';
import MjImage from '../components/mjml/mj-image';

const uuidv4 = require('uuid/v4');

export default {
  name: 'Generator',
  components: {
    MjButton,
    MjColumn,
    MjHeader,
    MjText,
    MjSection,
    MjImage,
    draggable,
  },
  data() {
    return {
      myArray: [
        { name: 'text', id: '1', type: 'mj-text', data: {} },
        { name: 'column', id: '2', type: 'mj-column', data: {} },
        { name: 'image', id: '3', type: 'mj-image', data: {} },
        { name: 'section', id: '4', type: 'mj-section', data: {} },
        { name: 'header', id: '5', type: 'mj-header', data: {} },
        { name: 'button', id: '6', type: 'mj-button', data: {} },
      ],
      myArray2: [
      ],
      drag: false,
    };
  },
  methods: {
    deleteElement(index) {
      this.myArray2.splice(index, 1);
    },
    insertComponent(obj) {
      const newEl = JSON.parse(JSON.stringify(obj));
      newEl.id = uuidv4();
      return newEl;
    },
  },
};
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
    -webkit-transition: box-shadow,transform .1s ease-in-out;
    -moz-transition: box-shadow,transform .1s ease-in-out;
    -o-transition: box-shadow,transform .1s ease-in-out;
    transition: box-shadow,transform .1s ease-in-out;
  }

  .element>i.actions.delete {
    top: 0;
    right: 0;
  }
  .element>i.actions.move {
    top: 0;
    left: 0;
  }
  .element>i.actions {
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
    line-height: 32px!important;
    height: 32px!important;
    color: #727272;
    border-radius: 50%;
    cursor: pointer;
    transition: background 280ms ease-out,color 280ms ease-out;
    width: 32px!important;
    text-align: center;
    -webkit-tap-highlight-color: rgba(0,0,0,0);
  }
  .element:hover>i.actions {
    opacity: 1;
  }


  .columns {
    display: flex;
    flex:1;
    overflow: auto;
    padding: 5px;
  }
  .main {
    flex: 1;
    order: 2;
    min-width: 600px;
    align-items: start;
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
</style>
