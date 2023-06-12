<template>


  <div>

    <b-table striped hover :items="items"></b-table>
  </div>
</template>


  <script>
  import axios from 'axios'
  export default{    
      data(){ 
          return { 
              items: [],
              newuser: {},
              postURL: 'http://127.0.0.1:5050'
              
          }
      },
      created(){ // este metodo se llama cuando se cra el componente
          // con esto hacemos una peticion a un server
          //with vue-resources
          //this.$http.post(this.postURL + '/users')
          //    .then(res => this.users = res.body);
          
          //with axios
          axios.post(this.postURL + '/users').then((res) => {  
                  //console.log(res);
                  this.items = res.data; 
                  console.log("MOSTRANDO TABLA USUARIO");           
          })
          .catch((error) => {
              console.log(error)
          })
      }
  
  }
  </script>
  
  <style>
      .users{
          background-color: #cccccc;
      }
  </style>