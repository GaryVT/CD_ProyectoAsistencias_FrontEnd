<template>
  
    <div>
      <h1 class="lines-effect">Eliminar Usuario</h1>

      <b-form @submit="onSubmit" @reset="onReset" v-if="show">

        <b-form-group id="input-group-1" label="Id del usuario a liminar:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.id_usuario"
          placeholder="Ingrese ID del usuario a eliminar"
          required
        ></b-form-input>
      </b-form-group>

  
        <ul></ul>
  

        <ul></ul>
  
        <b-button variant="primary" @click="llamarEndpointDelete">Eliminar Usuario</b-button>
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
          },
          show: true
        }
      },
      methods: {
        onSubmit(event) {
          event.preventDefault()
          //alert(JSON.stringify(this.form))  //Mostrar json
          alert('Eliminado correctamente')
        },
        onReset(event) {
          event.preventDefault()
          // Reset our form values
          this.form.id_usuario = ''
          // Trick to reset/clear native browser form validation state
          this.show = false
          this.$nextTick(() => {
            this.show = true
          })
        },
        llamarEndpointDelete() {
          const apiUrl = 'http://127.0.0.1:5050/user';
          const data = this.form
          axios.delete(apiUrl, {data})
            .then(response => {
              console.log('Llamada DELETE exitosa', response);
              alert("Eliminado con Exito")
              // Realiza las acciones adicionales después de una llamada exitosa
            })
            .catch(error => {
              console.error('Error en la llamada DELETE', error);
              // Maneja el error de alguna manera apropiada
            });
        }
  }
}
 
  </script>
  