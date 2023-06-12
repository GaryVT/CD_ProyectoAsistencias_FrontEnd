<template>
  <div>
    <h1 class="lines-effect">Ingresar Nuevo Usuario</h1>

    <form @submit.prevent="submitForm">

      <div class="form-group">
        <label for="dni">DNI:</label>
        <input type="text" id="dni" v-model="dni" class="form-control" required>
      </div>

      <div class="form-group">
        <label for="contrasena">contra:</label>
        <input type="password" id="contrasena" v-model="contrasena" class="form-control" required>
      </div>

      <div class="form-group">
        <label for="nombre">nombre:</label>
        <input type="text" id="nombre" v-model="nombre" class="form-control" required>
      </div>

      <div class="form-group">
        <label for="apellido">apellido:</label>
        <input type="text" id="apellido" v-model="apellido" class="form-control" required>
      </div>

      <div class="form-group">
        <label for="tipousuario">tipousuario:</label>
        <input type="text" id="tipousuario" v-model="tipousuario" class="form-control" required>
      </div>

        <ul></ul>
      <div class="form-group">
        <label for="fileInput">Imagen:</label>
        <input type="file" id="fileInput" ref="fileInput" class="form-control-file" required>
      </div>
      <ul></ul>
      <b-button type="submit" variant="primary">Crear Usuario</b-button>
    </form>
  </div>
</template>
  
  <script>
    import axios from 'axios'
    export default {
      data() {
        return {
          dni: '',
          contrasena: '',
          nombre: '',
          apellido: '',
          tipousuario: '',
          file: null
        }
      },
      methods: {
      submitForm() {
        const apiUrl = 'http://127.0.0.1:5050/user';
      const fileInput = this.$refs.fileInput;
      const file = fileInput.files[0];

      const formData = new FormData();
      formData.append('dni', this.dni);
      formData.append('contrasena', this.contrasena);
      formData.append('nombre', this.nombre);
      formData.append('apellido', this.apellido);
      formData.append('tipousuario', this.tipousuario);
      formData.append('file', file);

      axios.put(apiUrl, formData)
        .then(response => {
          console.log('Llamada POST exitosa', response.data);
          console.log(this.stringValue)
          console.log(response.data)
          alert("Usuario Creado!")
          // Realiza las acciones adicionales después de una llamada exitosa
        })
        .catch(error => {
          console.error('Error en la llamada POST', error);
          // Maneja el error de alguna manera apropiada
        });
    }
    }
    }
  </script>

