<template>
  <v-app dark>
    <router-view/>
  </v-app>
</template>

<script>
import {Component, Vue} from 'vue-property-decorator';

@Component
export default class App extends Vue {
}
</script>
