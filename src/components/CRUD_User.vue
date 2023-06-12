<template>

  
    <div class="vue1">
      
      
      <div class="d-flex justify-content-end button-container">
        <b-button v-b-modal.modal-1 variant="success" > <b-icon icon="plus-lg"></b-icon> Crear Usuario</b-button>
      </div>
  
      <b-modal id="modal-1" title="Crear Usuario">
        <form @submit.prevent="submitForm" enctype="multipart/form-data">
        
        <div class="form-group">
          <label for="nombreInput">Nombre</label>
          <input
            type="text"
            class="form-control"
            id="nombreInput"
            v-model="form.nombre"
            required
          >
        </div>
        <div class="form-group">
          <label for="apellidoInput">Apellido</label>
          <input
            type="text"
            class="form-control"
            id="apellidoInput"
            v-model="form.apellido"
            required
          >
        </div>

        <div class="form-group">
          <label for="tipousuarioInput">tipousuario</label>
          <input
            type="number"
            class="form-control"
            id="tipousuarioInput"
            v-model="form.tipousuario"
            required
          >
        </div>  
        <div class="form-group">
          <label for="contrasenaInput">Contraseña</label>
          <input
            type="password"
            class="form-control"
            id="contrasenaInput"
            v-model="form.contrasena"
            required
          >
        </div>        
        <div class="form-group">
            <label for="dniInput">DNI</label>
            <input
              type="text"
              class="form-control"
              id="dniInput"
              v-model="form.dni"
              required
            >
        </div>
  
        <div class="form-group">
          <label for="fileInput">file (PNG)</label>
          <input
            type="file"
            class="form-control-file"
            id="fileInput"
            accept="image/jpg"    
            @change="handleFileUpload"

            required
          >
        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
      </form>
      
      </b-modal>
      
      
      <b-modal id="modal-2" title="Editar Usuario">
        <form @submit.prevent="patchUserData" enctype="multipart/form-data">
  
        <div class="form-group">
          <label for="idInput">ID</label>
          <p>{{ form.id }}</p>
        </div>
  

        <div class="form-group">
          <label for="nombreInput">Nombre</label>
          <input
            type="text"
            class="form-control"
            id="nombreInput"
            v-model="form.nombre"
            required
          >
        </div>
        <div class="form-group">
          <label for="apellidoInput">Apellido</label>
          <input
            type="text"
            class="form-control"
            id="apellidoInput"
            v-model="form.apellido"
            required
          >
        </div>
        <div class="form-group">
          <label for="tipousuarioInput">tipousuario</label>
          <input
            type="number"
            class="form-control"
            id="tipousuarioInput"
            v-model="form.tipousuario"
            required
          >
        </div>
        <div>
        <div class="mt-3">Selected: <strong>{{ selected }}</strong></div>
        <b-form-select v-model="selected" :options="options" size="sm" class="mt-3"></b-form-select>
        </div>
        
        <div class="form-group">
          <label for="contrasenaInput">contrasena</label>
          <input
            type="password"
            class="form-control"
            id="contrasenaInput"
            v-model="form.contrasena"
            required
          >
        </div>
        
        <div class="form-group">
            <label for="dniInput">DNI</label>
            <input
              type="text"
              class="form-control"
              id="dniInput"
              v-model="form.dni"
              required
            >
        </div>
  
        <div class="form-group">
          <label for="fileInput">file (PNG)</label>
          <input
            type="file"
            class="form-control-file"
            id="fileInput"
            accept="image/jpg"
            @change="handleFileUpload"
            required
         >
        </div>
        <button type="submit" class="btn btn-primary">Actualizar</button>
      </form>
      
      </b-modal>
  
      <template>
        <div class="overflow-auto">
          <b-pagination
            v-model="currentPage"
            :total-rows="rows"
            :per-page="7"
            aria-controls="my-table"
          ></b-pagination>
  
          <p class="mt-3">Current Page: {{ currentPage }}</p>
  
          <b-table responsive 
          class="myTable"
          label-sort-asc=""
          label-sort-desc=""
          label-sort-clear=""
          :sticky-header="stickyHeader" 
          :items="responseData" 
          :fields="fields"
          :per-page="rwr"
          :current-page="currentPage">
          
          
          <template #cell(acciones)="row">

              <b-button variant="danger" @click="deleteUser(row.item.id)"><b-icon icon="trash"></b-icon> </b-button>
  
              <b-button v-b-modal.modal-2 variant="primary" @click="openEditModal(row.item)"><b-icon icon="pencil"></b-icon> </b-button>
  
              <!-- As `row.showDetails` is one-way, we call the toggleDetails function on @change -->
              
          </template>
          
          </b-table>
        </div>
      </template>
     
  
      
      
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    computed: {
        rows() {
          return this.items.length
        }
    },
  
    data() {
      return {
        selected: null,
        options: [
          { value: null, text: 'Seleccione una opción', disabled: true },
          { value: '1', text: '1 - Administrador' },
          { value: '2', text: '2 - Alumno' },
          { value: { C: '3PO' }, text: 'This is an option with object value' },
          { value: '3', text: '3 - Docente' },
        ],



        responseData: null,
        stickyHeader: true,
        showModal: false,
        perPage: "rows",
        currentPage: 1,
        form: {
          contrasena: '',
          contrasena: '',
          nombre: '',
          apellido: '',
          tipousuario: '',
          dni: '',
          file: null
        },
  
        fields: [
            {
              key: 'id',
              sortable: true,
              
            },
            {
              key: 'nombre',
              sortable: true
            },
            {
              key: 'apellido'
            },
            {
              key: 'contrasena',
              sortable: true,
              type:'password'
            },
            {
              key: 'tipousuario',
              sortable: true
            },            
            {
              key: 'dni',
              sortable: true
            },/*
            {
              key: 'fotoSistema',
              maxCharacters: 10,
              sortable: true
            },*/
            {
              key: 'acciones'
            }
        ],
  
        items: [
            { age: 40, first_name: 'Dickerson', last_name: 'Macdonald' },
            { age: 21, first_name: 'Larsen', last_name: 'Shaw' },
            { age: 89, first_name: 'Geneva', last_name: 'Wilson' },
            { age: 38, first_name: 'Jami', last_name: 'Carney' }
        ]
      };
    },
  
    created() {
      axios.post('http://127.0.0.1:5050/users')
        .then(response => {
          this.responseData = response.data.map(user => {
          return {
            id:user.id_usuario,
            contrasena: user.contrasena,
            contrasena: user.contrasena,
            apellido: user.apellido,
            nombre: user.nombre,
            tipousuario: user.tipousuario,
            file : user.file,
            dni : user.dni,
            fotoSistema : user.fotoSistema
          };
        });
        })
        .catch(error => {
          // Manejar el error
          console.error(error);
        });
    },
  
    methods: {
        truncateText(text, maxLength) {
      if (text.length > maxLength) {
        return text.slice(0, maxLength) + '...';
      }
      return text;
    },
      // Resto de los métodos del componente
      openEditModal(item) {
        this.form = { ...item };
        this.showModal = true;
      },
  
      deleteUser(id) {
      axios.delete('http://127.0.0.1:5050/user', { data: { id_usuario: id } })
        .then(response => {
          // Realizar cualquier acción adicional después de borrar el usuario
          console.log(response.data);
          
        })
        .catch(error => {
          // Manejar el error
          console.error(error);
        });
      },
  
      patchUserData() {
        
        const formData = new FormData();
        formData.append('id_usuario', this.form.id);
        formData.append('nombre', this.form.nombre);
        formData.append('apellido', this.form.apellido);
        formData.append('tipousuario', this.form.tipousuario);
        formData.append('contrasena', this.form.contrasena);
        formData.append('dni', this.form.dni);
        formData.append('file', this.form.file);
        
        axios.patch('http://127.0.0.1:5050/user', formData)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id = '';
            this.form.contrasena = '';
            this.form.nombre = '';
            this.form.apellido = '';
            this.form.tipousuario = '';
            this.form.dni = '';
            this.form.file = null;
            alert("Actualizado")
          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
          });
      },
  
      submitForm() {
        const formData = new FormData();
        formData.append('dni', this.form.dni);
        formData.append('contrasena', this.form.contrasena);
        formData.append('nombre', this.form.nombre);
        formData.append('apellido', this.form.apellido);
        formData.append('tipousuario', this.form.tipousuario);
        formData.append('file', this.form.file);
        
        axios.put('http://127.0.0.1:5050/user', formData)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id = '';
            this.form.contrasena = '';
            this.form.nombre = '';
            this.form.apellido = '';
            this.form.tipousuario = '';
            this.form.dni = '';
            this.form.file = null;
          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
          });
      },
      
      handleFileUpload(event) {
        const file = event.target.files[0];
        this.form.file = file;
      },
  
      openEditModal(item) {
        this.form = { ...item };
        this.showModal = true;
      }
    }
  } 
  </script>
  
  <style>
  .text-truncate {
  max-width: 15px; /* Ajusta el ancho máximo según tus necesidades */
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

  .sticky-header-table thead {
    position: sticky;
    top: 0;
    background-color: #f8f9fa;
    z-index: 1; /* Asegura que el encabezado esté por encima del contenido de la tabla */
  }
  
  .button-container {
    padding: 20px;
  }
  
  .myTable thead {
    background-color: black;
  }
  
  </style>