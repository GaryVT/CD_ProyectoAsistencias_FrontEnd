<template>

  
    <div class="vue1">
      
      
      <div class="d-flex justify-content-end button-container">
        <b-button v-b-modal.modal-5 variant="success" > <b-icon icon="plus-lg"></b-icon> Crear Tipo Usuario</b-button>
      </div>
  
      <b-modal id="modal-5" title="Crear Curso">
        <form @submit.prevent="submitform" enctype="text/plain">
        <div class="form-group">
          <label for="id_docenteInput">ID Docente</label>
          <input
            type="text"
            class="form-control"
            id="id_docenteInput"
            v-model="form.id_docente"
            required
          >
        </div>
        <div class="form-group">
          <label for="nombre_cursoInput">Nombre Curso</label>
          <input
            type="text"
            class="form-control"
            id="nombre_cursoInput"
            v-model="form.nombre_curso"
            required
          >
        </div>
        <div class="form-group">
          <label for="descripcionInput">Descripcion</label>
          <input
            type="text"
            class="form-control"
            id="descripcionInput"
            v-model="form.descripcion"
            required
          >
        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
      </form>
      
      </b-modal>
      
      
      <b-modal id="modal-2" title="Editar Curso">
        <form @submit.prevent="patchcursoData" enctype="text/plain">
  
        <div class="form-group">
          <label for="id_cursoInput">ID</label>
          <p>{{ form.id_curso }}</p>
        </div>
  

        <div class="form-group">
          <label for="nombre_cursoInput">nombre_curso</label>
          <input
            type="text"
            class="form-control"
            id="nombre_cursoInput"
            v-model="form.nombre_curso"
            required
          >
        </div>
        <div class="form-group">
          <label for="descripcionInput">descripcion</label>
          <input
            type="text"
            class="form-control"
            id="descripcionInput"
            v-model="form.descripcion"
            required
          >
        </div>
        <div class="form-group">
          <label for="id_docenteInput">id_docente</label>
          <input
            type="text"
            class="form-control"
            id="id_docenteInput"
            v-model="form.id_docente"
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

              <b-button variant="danger" @click="deletecurso(row.item.id_curso)"><b-icon icon="trash"></b-icon> </b-button>
  
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
        responseData: null,
        stickyHeader: true,
        showModal: false,
        perPage: "rows",
        currentPage: 1,
        form: {
          id_docente: '',
          nombre_curso: '',
          descripcion: ''
        },
  
        fields: [
            {
              key: 'id_curso',
              sortable: true,
              
            },
            {
              key: 'nombre_curso',
              sortable: true
            },
            {
              key: 'descripcion'
            },
            {
              key: 'id_docente',
              sortable: true,
              
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
      axios.post('http://127.0.0.1:5050/cursos')
        .then(response => {
          this.responseData = response.data.map(curso => {
          return {
            id_curso:curso.id_curso,
            id_docente: curso.id_docente,
            descripcion: curso.descripcion,
            nombre_curso: curso.nombre_curso,
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
  
      deletecurso(id) {
      axios.delete('http://127.0.0.1:5050/curso', { data: { id_curso: id } })
        .then(response => {
          // Realizar cualquier acción adicional después de borrar el usuario
          console.log(response.data);
          
        })
        .catch(error => {
          // Manejar el error
          console.error(error);
        });
      },
  
      patchcursoData() {
        const data = this.form

        axios.patch('http://127.0.0.1:5050/curso',  data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id_curso = '';
            this.form.id_docente = '';
            this.form.nombre_curso = '';
            this.form.descripcion = '';
            alert("Actualizado")
          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
          });
      },
  
      submitform() {
        const data = this.form
       
        axios.put('http://127.0.0.1:5050/curso', data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id_curso = '';
            this.form.id_docente = '';
            this.form.nombre_curso = '';
            this.form.descripcion = '';
            alert("Creado")

          })
          .catch(error => {
            console.error(error); // Maneja los errores de la petición
            alert("Error")
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