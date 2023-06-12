<template>

  
    <div class="vue1">
      
      
      <div class="d-flex justify-content-end button-container">
        <b-button v-b-modal.modal6 variant="success" > <b-icon icon="plus-lg"></b-icon> Crear Docente</b-button>
      </div>
  
      <b-modal id="modal6" title="Crear docente">
        <form @submit.prevent="submitform" enctype="text/plain">
        <div class="form-group">

            
            <b-form-group id="input-group-4" label="Profesores Contratados:" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="form.id_usuario"
          :options="contratados"
          required
        ></b-form-select>
      </b-form-group>





        </div>
        <div class="form-group">

        </div>
        <div class="form-group">

            <b-form-group id="input-group-5" label="Tipo de Usuario:" label-for="input-5">
        <b-form-select
          id="input-4"
          v-model="form.tipousr_id"
          :options="tipousr_id"
          required
        ></b-form-select>
      </b-form-group>

      <div class="form-group">
          <label for="tipo_docenteInput">tipo_docente</label>
          <input
            type="text"
            class="form-control"
            id="tipo_docenteInput"
            v-model="form.tipo_docente"
            required
          >
        </div>

        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
      </form>
      
      </b-modal>
      
      
      <b-modal id="modal-66" title="Editar docente">
        <form @submit.prevent="patchdocenteData" enctype="text/plain">
  
        <div class="form-group">
          <label for="id_docenteInput">ID</label>
          <p>{{ form.id_docente }}</p>
        </div>        
        <div class="form-group">

            
        <b-form-group id="input-group-4" label="Profesores Disponibles:" label-for="input-4">
        <b-form-select
            id="input-4"
            v-model="form.id_usuario"
            :options="contratados"
            required
            ></b-form-select>
        </b-form-group>




        </div>
        <div class="form-group">


        <b-form-group id="input-group-5" label="Tipo de Usuario:" label-for="input-5">
            <b-form-select
            id="input-5"
            v-model="form.tipousr_id"
            :options="tipousr_id"
            required
            ></b-form-select>
         </b-form-group>
            
        </div>
        <div class="form-group">
          <label for="tipodocenteInput">Tipo Docente</label>
          <input
            type="text"
            class="form-control"
            id="tipodocenteInput"
            v-model="form.tipo_docente"
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

              <b-button variant="danger" @click="deletedocente(row.item.id_docente)"><b-icon icon="trash"></b-icon> </b-button>
  
              <b-button v-b-modal.modal-66 variant="primary" @click="openEditModal(row.item)"><b-icon icon="pencil"></b-icon> </b-button>
  
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
        responseData: null,
        stickyHeader: true,
        showModal: false,
        perPage: "rows",
        currentPage: 1,
        form: {
            id_usuario: '',
            tipousr_id: '',
            tipo_docente: '',
        },
        contratados: [{ text: 'Elije uno', value: null }, { text: 'Vicente Machaca', value: 6 }, { text: 'Abel Carrasco', value: 6 }, { text: 'Mario Vargas', value: 3}],
        tipousr_id: [{ text: 'Elije uno', value: null }, { text: 'Admin', value: 1 }, { text: 'Alumno', value: 2 }, { text: 'Docente', value: 3}],

  
        fields: [
            {
              key: 'id_docente',
              sortable: true,
              
            },
            {
              key: 'dni',
              sortable: true
            },
            {
              key: 'apellido'
            },
            {
              key: 'nombre',
              sortable: true,
              
            },

            {
              key: 'tipo_docente',
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
      axios.post('http://127.0.0.1:5050/docentes')
        .then(response => {
          this.responseData = response.data.map(docente => {
          return {
            id_docente:docente.id_docente,
            nombre: docente.nombre,
            apellido: docente.apellido,
            dni: docente.dni,
            tipo_docente: docente.tipo_docente,
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
  
      deletedocente(id) {
      axios.delete('http://127.0.0.1:5050/docente', { data: { id_docente: id } })
        .then(response => {
          // Realizar cualquier acción adicional después de borrar el usuario
          console.log(response.data);
          alert("Eliminado")
        })
        .catch(error => {
          // Manejar el error
          console.error(error);
        });
      },
  
      patchdocenteData() {
        const data = this.form

        axios.patch('http://127.0.0.1:5050/docente',  data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo

            alert("Actualizado")
          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
          });
      },
  
      submitform() {
        const data = this.form
       
        axios.put('http://127.0.0.1:5050/docente', data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id_docente = '';
            this.form.nombre = '';
            this.form.dni = '';
            this.form.apellido = '';
            alert("Creado")

          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
            alert(error)
            console.log(this.form)
          });
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