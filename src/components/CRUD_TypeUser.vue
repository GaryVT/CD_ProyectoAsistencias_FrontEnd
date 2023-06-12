<template>

  
    <div class="vue1">
      
      
      <div class="d-flex justify-content-end button-container">
        <b-button v-b-modal.modal-11 variant="success" > <b-icon icon="plus-lg"></b-icon> Crear Tipo Usuario</b-button>
      </div>
  
      <b-modal id="modal-11" title="Crear Tipo Usuario">
        <form @submit.prevent="submitform" enctype="text/plain">
        
        <div class="form-group">
          <label for="nombre_tusrInput">Nombre Tipo Usuario</label>
          <input
            type="text"
            class="form-control"
            id="nombre_tusrInput"
            v-model="form.nombre_tusr"
            required
          >
        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
      </form>
      
      </b-modal>
      
      
      <b-modal id="modal-22" title="Editar Usuario">
        <form @submit.prevent="patchtypeuserData" enctype="text/plain">
  
        <div class="form-group">
          <label for="idInput">ID</label>
          <p>{{ form.tipousr_id }}</p>
        </div>
        <div class="form-group">
          <label for="nombre_tusrInput">Nombre Tipo Usuario</label>
          <input
            type="text"
            class="form-control"
            id="nombre_tusrInput"
            v-model="form.nombre_tusr"
            required
          >
        </div>
        <ul></ul>
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
          :per-page="1000"
          :current-page="currentPage">
          
          
          <template #cell(acciones)="row">

              <b-button variant="danger" @click="deletetypeuser(row.item.tipousr_id)"><b-icon icon="trash"></b-icon> </b-button>
  
              <b-button v-b-modal.modal-22 variant="primary" @click="openEditModal(row.item)"><b-icon icon="pencil"></b-icon> </b-button>
  
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
        form: {
            'nombre_tusr': '',
		        'tipousr_id': '',
          },
        selected: null,

        responseData: null,
        stickyHeader: true,
        showModal: false,
        perPage: "rows",
        currentPage: 1,
        form: {
          tipousr_id: '',
          nombre_tusr: '',
        },
  
        fields: [
            {
              key: 'tipousr_id',
              sortable: true,
              
            },
            {
              key: 'nombre_tusr',
              sortable: true
            },
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
      axios.post('http://127.0.0.1:5050/tipo_usuarios')
        .then(response => {
          this.responseData = response.data.map(typeuser => {
          return {
            tipousr_id:typeuser.tipousr_id,
            nombre_tusr: typeuser.nombre_tusr,
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
  
      deletetypeuser(id) {
        
        axios.delete('http://127.0.0.1:5050/tipo_usuario',  { data: { tipousr_id: id } } )
        .then(response => {
          // Realizar cualquier acción adicional después de borrar el usuario
          console.log(response.data);
          
        })
        .catch(error => {
          // Manejar el error
          console.error(error);
        });
      },
  
      patchtypeuserData() {
        const data = this.form

        axios.patch('http://127.0.0.1:5050/tipo_usuario',  data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.nombre_tusr = '';
            this.form.tipousr_id = '';
            alert("Actualizado")
          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
          });
      },
  
      submitform() {
        const data = this.form
       
        axios.put('http://127.0.0.1:5050/tipo_usuario', data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.tipousr_id = '';
            this.form.nombre_tusr = '';
            alert("creado")

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