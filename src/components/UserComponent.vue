<template>
    <div class='users'>
        <h2>Users</h2>
        <ul>
            <li v-for='user in users'> 
                {{user.nombre}} - {{ user.apellido }} 
                <button v-on:click='delete_user(user)'>x</button>
            </li>
        </ul>
    
        <form v-on:submit='add_user'>
        <!-- <form v-on:submit.prevent='addUser'> --> <!-- con esto prevenimo el comportamiento por defecto, la recarga de pagina al submit--> 
            <input type='text' v-model='newuser.title' placeHolder='title'>
            <input type='text' v-model='newuser.description' placeHolder='description'>
            <button type='submit'> Add </button>
        </form> <br>
    </div>
    </template>
    
    <script>
    import axios from 'axios'
    export default{    
        data(){ 
            return { 
                users: [],
                newuser: {},
                postURL: 'http://127.0.0.1:5050'
                
            }
        },
        methods:{
            add_user(e){            
                e.preventDefault(); // cancela el comportamiento por defecto, en este caso evitar que se vuelva a cargar la pagina luego del submit
                var config_request={
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }
                // with vue-resources            
                /*this.$http.post(this.postURL + '/create_user', this.newuser, config_request)
                    .then(res => {
                        this.users.push(res.body);                    
                    }, res => {
                        console.error(res.body);
                    });  */
    
                // with axios
                axios.post(this.postURL + '/create_user', this.newuser, { config_request })
                    .then(res => {                                         
                        this.users.push(res.data);
                        console.log(res.data)        ;
                    })
                    .catch((error) => {
                        console.log(error)
                    });
    
                this.newuser = {};
            },
            deleteuser(user){  
                var config_request={
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }          
                axios.post(this.postURL + '/delete_user', {id:user.id}, { config_request })
                    .then(res => {                      
                        this.users.splice(this.users.indexOf(user), 1);
                        console.log(res.data);
                    })
                    .catch((error) => {
                        console.log(error)
                    });  
            }
        },
        created(){ // este metodo se llama cuando se cra el componente
            // con esto hacemos una peticion a un server
            //with vue-resources
            //this.$http.post(this.postURL + '/users')
            //    .then(res => this.users = res.body);
            
            //with axios
            axios.post(this.postURL + '/users').then((res) => {  
                    console.log(res);
                    this.users = res.data;            
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