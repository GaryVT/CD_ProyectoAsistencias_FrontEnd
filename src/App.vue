<template>
  
  <div id="app">

    <img src="./assets/UHorizontal.png">

    <ul></ul>
    <h1 class="lines-effect">Usuarios</h1>
    <ul></ul>
    <b-button variant="success" @click="mostrarComponente">Mostrar Usuarios</b-button>
    <b-button variant="danger" @click="ocultarComponente">Ocultar Usuarios</b-button>
    <div v-if="mostrar">
      <Tabla1></Tabla1>
    </div>
    <ul></ul>
    <h1 class="lines-effect">Tipo de Usuarios</h1>
    <ul></ul>
    <b-button variant="success" @click="mostrarComponente2">Mostrar Tipo Usuarios</b-button>
    <b-button variant="danger" @click="ocultarComponente2">Ocultar Tipo Usuarios</b-button>
    <div v-if="mostrar2">
      <Tabla2></Tabla2>
    </div>
    <ul></ul>
    <h1 class="lines-effect">Cursos</h1>
    <ul></ul>
    <b-button variant="success" @click="mostrarComponente3">Mostrar Cursos</b-button>
    <b-button variant="danger" @click="ocultarComponente3">Ocultar Cursos</b-button>
    <div v-if="mostrar3">
      <Tabla3></Tabla3>
    </div>
    <ul></ul>
    <h1 class="lines-effect">Docentes</h1>
    <ul></ul>
    <b-button variant="success" @click="mostrarComponente4">Mostrar Docentes</b-button>
    <b-button variant="danger" @click="ocultarComponente4">Ocultar Docentes</b-button>
    <div v-if="mostrar4">
      <Tabla4></Tabla4>
    </div>
    <ul></ul>
    <h1 class="lines-effect">Horario</h1>
    <ul></ul>
    <b-button variant="success" @click="mostrarComponente5">Mostrar Horario</b-button>
    <b-button variant="danger" @click="ocultarComponente5">Ocultar Horario</b-button>
    <div v-if="mostrar5">
      <Tabla5></Tabla5>
    </div>

    </div>

</template>

<script>
import Tabla1 from './components/CRUD_User'
import Tabla2 from './components/CRUD_TypeUser'
import Tabla3 from './components/CRUD_Curso'
import Tabla4 from './components/CRUD_Docente'
import Tabla5 from './components/CRUD_Horario'


export default {
  data (){
    return {mostrar: false,
            mostrar2: false,
            mostrar3: false,
            mostrar4: false,
            mostrar5: false
    
    };
  },

  name: 'App',
  components: {
    Tabla1,
    Tabla2,
    Tabla3,
    Tabla4,
    Tabla5

  },
  methods: {
    mostrarComponente() {
      this.mostrar = true;
    },
    ocultarComponente() {
      this.mostrar = false;
    },
    mostrarComponente2() {
      this.mostrar2 = true;
    },
    ocultarComponente2() {
      this.mostrar2 = false;
    },
    mostrarComponente3() {
      this.mostrar3 = true;
    },
    ocultarComponente3() {
      this.mostrar3 = false;
    },
    mostrarComponente4() {
      this.mostrar4 = true;
    },
    ocultarComponente4() {
      this.mostrar4 = false;
    },
    mostrarComponente5() {
      this.mostrar5 = true;
    },
    ocultarComponente5() {
      this.mostrar5 = false;
    }
  }
}
</script>

<style>
@font-face {
  font-family: 'MyFont';
  src: url('https://fonts.googleapis.com/css?family=Dancing+Script');
}

#app {  
  font-family: 'MyFont','Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 350px; /* Margen izquierdo de 20 píxeles */
  margin-right: 350px; /* Margen derecho de 20 píxeles */

  align-items: center; /* Centra verticalmente el contenido */
  justify-content: center; /* Centra horizontalmente el contenido */
  height: 100vh; /* Establece la altura del contenedor principal */
}
#app img {
  max-width: 100%; /* Ajusta el tamaño máximo de la imagen */
}


#app h1 {
  display: inline-block;
  position: relative;
  color: black;
  font:italic;
}
h1::after,h1::before{
  content: '';
  position: absolute;
  width: 135px;
  height: 3px;
  top: 0.7cm;
  background-color: currentColor;
  transform: scale(0);
  transition: all 0.2s ease-in;

}
h1:hover::before, h1:hover::after{
  transform: scale(1);
}
h1::before{
  left: -135px;
}
h1::after{
  right: -135px;
}




</style>


