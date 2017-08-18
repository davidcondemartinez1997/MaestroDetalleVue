<template>
  <div id="form">
    <h1>Formulario</h1>
    <div>
      <label>Nombre:</label>
      <input type="text" id="nombre" name="nombre" v-bind:value="person.Nombre"/>   
    </div>
    <div>
      <label>Apellidos:</label>
      <input type="text" id="apellidos" name="apellidos" v-bind:value="person.Apellidos"/>   
    </div>

    <div>
      <label>Edad:</label>
      <input type="number" id="edad" name="edad"v-bind:value="person.Edad" />   
    </div>
    

    <input type="button" id="submit" value="Enviar" v-on:click="enviar"/>
  </div>
</template>

<script>
  import axios from 'axios';
  import {EventBus} from './EventBus.js';
  export default {
    name: 'app',
    data () {
      return {
         person: {}
      }
    },
    methods: {
      enviar: function(){
        let data = {
          Nombre: document.getElementById("nombre"),
          Apellidos: document.getElementById("apellidos"),
          Edad: document.getElementById("edad"),
          ID: this.person.ID
        }

        axios.put('http://10.0.2.2:50000/api/Personas/' + data.ID, data)
        .then(response => {
          EventBus.$emit("update", this.person);
        })
      }
    },
    created() {
      this.person = this.$parent.person;
    }

  }
</script>

<style>
  *{
    padding: 5px;
  }
</style>
