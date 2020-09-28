<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
      <a href="https://cli.vuejs.org" target="_blank" rel="noopener">vue-cli documentation</a>.
    </p>

    <draggable v-model="myArray">
      <transition-group>
        <div v-for="element in myArray" :key="element.id">
          {{element.name}}
        </div>
      </transition-group>
    </draggable>

  </div>
</template>

<script>
  import draggable from 'vuedraggable';

  export default {
    name: 'HelloWorld',
    props: {
      msg: String,
    },
    components: {
      draggable,
    },
    data: () => ({
      myArray: [
        {id: 1, name: 'test1'},
        {id: 2, name: 'test2'},
        {id: 3, name: 'test3'},
        {id: 4, name: 'test4'},
        {id: 5, name: 'test5'},
        {id: 6, name: 'test6'},
      ],
    }),
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h3 {
    margin: 40px 0 0;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
