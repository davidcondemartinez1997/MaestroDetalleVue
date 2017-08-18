<template>
  <div id="app">
    <ul v-if="people && people.length">
      <li v-for="person of people" v-on:click="detail" v-bind:id="person.ID">
        {{person.Nombre}} {{person.Apellidos}}
      </li>
    </ul>
    <div id="form" v-on:update="init"></div>
  </div>

</template>

<script>
  import axios from 'axios';
  import Form from './Form.vue'
  import {EventBus} from './EventBus.js';
  import Vue from 'vue'

  export default {
    name: 'app',
    data () {
      return {
        people: undefined
      }
    },
    methods: {
      detail: function (e) {
        let id = e.target.id;
        this.people.forEach((p, index) => {
          if(p.ID == id){
            new Vue({
              el: '#form',
              render: h => h(Form),
              data: {
                person: p
              },
            });
          }
        });
      },
      init: function(){
        axios.get('http://10.0.2.2:50000/api/Personas/')
        .then(response => {
          this.people = response.data;
        })
      }

    },
    created() {
      this.init();
      EventBus.$on('update', ((person) => {
        this.people.forEach((p, index) => {
          if(p.ID == person.ID){
            this.people[index] = person;
          }
        });
      }));

    }

  }
</script>

<style>
  *{
    padding: 5px;
  }

  li {
    list-style: none;
    border: 1px solid black;
    width: 200px;
  }
</style>
