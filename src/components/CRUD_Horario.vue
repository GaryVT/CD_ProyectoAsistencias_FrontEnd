<template>
    <div class="vue1">   
      <div class="d-flex justify-content-end button-container">
        <b-button v-b-modal.modal6 variant="success" > <b-icon icon="plus-lg"></b-icon> Crear horario</b-button>
      </div>
      <b-modal id="modal6" title="Crear horario">
        <form @submit.prevent="submitform" enctype="text/plain">
        <div class="form-group">         
            <b-form-group id="input-group-4" label="Cursos Disponibles:" label-for="input-4">
        <b-form-select
          id="input-4"
          v-model="form.id_curso"
          :options="cursos"
          required
        ></b-form-select>
      </b-form-group>
        </div>
        <div class="form-group">

        </div>
        <div class="form-group">

            <b-form-group id="input-group-5" label="Dia:" label-for="input-5">
        <b-form-select
          id="input-5"
          v-model="form.dia"
          :options="dia"
          required
        ></b-form-select>
      </b-form-group>

      <div class="form-group">
          <label for="aulaInput">aula</label>
          <input
            type="text"
            class="form-control"
            id="aulaInput"
            v-model="form.aula"
            required
          >
        </div>

        <div class="form-group">
          <label for="hora_inicioInput">Hora Inicio</label>
          <b-time v-model="form.hora_inicio" type="time" show-seconds locale="en" required>
         </b-time>
        </div>

        <div class="form-group">
          <label for="hora_finInput">Hora Fin</label>
          <b-time v-model="form.hora_fin" type="time" show-seconds locale="en" required>
         </b-time>
        </div>

        </div>
        <button type="submit" class="btn btn-primary">Enviar</button>
      </form>
      
      </b-modal>
      
      
      <b-modal id="modal-66" title="Editar horario">
        <form @submit.prevent="patchhorarioData" enctype="text/plain">
  
        <div class="form-group">
          <label for="id_horarioInput">ID</label>
          <p>{{ form.id_horario }}</p>
        </div>        
        <div class="form-group">

            
        <b-form-group id="input-group-6" label="Cursos Disponibles:" label-for="input-6">
            <b-form-select
          id="input-6"
          v-model="form.id_curso"
          :options="cursos"
          required
        ></b-form-select>
        </b-form-group>




        </div>
        <div class="form-group">



            <b-form-group id="input-group-7" label="Dia:" label-for="input-7">
        <b-form-select
          id="input-7"
          v-model="form.dia"
          :options="dia"
          required
        ></b-form-select>
      </b-form-group>
            
        </div>
        <div class="form-group">
          <label for="tipohorarioInput">Aula</label>
          <input
            type="text"
            class="form-control"
            id="tipohorarioInput"
            v-model="form.aula"
            required
          >
        </div>

        <div class="form-group">
          <label for="hora_inicioInput">Hora Inicio</label>
          <b-time v-model="form.hora_inicio" type="time" show-seconds locale="en" required>
         </b-time>
        </div>

        <div class="form-group">
          <label for="hora_finInput">Hora Fin</label>
          <b-time v-model="form.hora_fin" type="time" show-seconds locale="en" required>
         </b-time>
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

              <b-button variant="danger" @click="deletehorario(row.item.id_horario)"><b-icon icon="trash"></b-icon> </b-button>
  
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
            id_curso: '',
            hora_inicio: '',
            hora_fin: '',
            dia: '',
            aula: '',
        },
        cursos: [{ text: 'Elije uno', value: null },
                 { text: 'Construccion Software', value: 2 },
                 { text: 'Compiladores', value: 3 },
                 { text: 'FLP', value: 4 },
                 { text: 'Religions', value: 13}],
        
        dia: [{ text: 'Elije uno', value: null },
                 { text: 'Lunes', value: 'Lunes' },
                 { text: 'Martes', value: 'Martes' },
                 { text: 'Miercoles', value: 'Miercoles' },
                 { text: 'Jueves', value: 'Jueves' },
                 { text: 'Viernes', value: 'Viernes' },
                 { text: 'Sabado', value: 'Sabado' },
                 { text: 'Domingo', value: 'Domingo'}],
  
        fields: [
            {
              key: 'id_horario',
              sortable: true,
              
            },
            {
              key: 'dia',
              sortable: true
            },
            {
              key: 'hora_inicio'
            },
            {
              key: 'hora_fin',
              sortable: true,
              
            },

            {
              key: 'aula',
              sortable: true
            },
            {
              key: 'id_curso',
              sortable: true
            },
            {
              key: 'acciones'
            }
        ],
  
        items: []
      };
    },
  
    created() {
      axios.post('http://127.0.0.1:5050/horarios')
        .then(response => {
          this.responseData = response.data.map(horario => {
          return {
            id_horario:horario.id_horario,
            hora_fin: horario.hora_fin,
            hora_inicio: horario.hora_inicio,
            dia: horario.dia,
            aula: horario.aula,
            id_curso: horario.id_curso,
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
  
      deletehorario(id) {
      axios.delete('http://127.0.0.1:5050/horario', { data: { id_horario: id } })
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
  
      patchhorarioData() {
        const data = this.form

        axios.patch('http://127.0.0.1:5050/horario',  data)
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
       
        axios.put('http://127.0.0.1:5050/horario', data)
          .then(response => {
            console.log(response.data); // Maneja la respuesta de la API
            // Restablece los campos del formulario después de enviarlo
            this.form.id_horario = '';
            this.form.hora_fin = '';
            this.form.dia = '';
            this.form.hora_inicio = '';
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
      },


      setNow() {
        const now = new Date()
        // Grab the HH:mm:ss part of the time string
        this.value = now.toTimeString().slice(0, 8)
      },
      clearTime() {
        this.value = ''
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