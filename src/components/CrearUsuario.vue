<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">


      <b-form-group id="input-group-1" label="Tu DNI:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.dni"
          placeholder="Ingrese DNI del usuario"
          required
        ></b-form-input>
      </b-form-group>

      <ul></ul>

      <b-form-group id="input-group-2" label="Tu nombre:" label-for="input-2">
        <b-form-input
          id="input-2"
          v-model="form.nombre"
          placeholder="Ingrese nombre"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-3" label="Tu Apellido:" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.apellido"
          placeholder="Ingrese Apellido"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-4" label="Tipo de Usuario:" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="form.tipousuario"
          :options="tipousuarios"
          required
        ></b-form-select>
      </b-form-group>

      <ul></ul>

      <b-form-group id="input-group-5" label="Tu Constraseña:" label-for="input-5" >
        <b-form-input
          id="input-5"
          v-model="form.contrasena"
          aria-describedby="input-live-help input-live-feedback"
          placeholder="Ingrese Constraseña"
          type="password"
        ></b-form-input>
      </b-form-group>

      <ul></ul>


    <b-button variant="success" @click="llamarEndpointPut">Llamar Endpoint PUT</b-button>

      <b-button type="submit" variant="primary">Crear Usuario</b-button>
      <b-button type="reset" variant="danger">Resetear Datos</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card>
  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    data() {
      return {
          form: {
            id_usuario: '',
            dni: '',
            contrasena: '',
            nombre: '',
            apellido: '',
            tipousuario: null,
          },
          tipousuarios: [{ text: 'Elije uno', value: null }, { text: 'Admin', value: 1 }, { text: 'Alumno', value: 2 }, { text: 'Docente', value: 3}],
          show: true,
          postURL: 'http://127.0.0.1:5050'
        }
      },
    methods: {
      onSubmit(event) {
        event.preventDefault()
        //alert(JSON.stringify(this.form))  //Mostrar json
        alert('Creado correctamente')
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.nombre = ''
        this.form.dni = ''
        this.form.apellido = ''
        this.form.tipousuario = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      },
      llamarEndpointPut(form) {
            const apiUrl = 'http://127.0.0.1:5050/userr';
            const data = this.form
            axios.put(apiUrl, data)
            .then(response => {
                console.log('Llamada PUT exitosa', response);
                // Realiza las acciones adicionales después de una llamada exitosa
            })
            .catch(error => {
                console.error('Error en la llamada PATCH', error);
                // Maneja el error de alguna manera apropiada
            });
        }
      }
    }
  





</script>