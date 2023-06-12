<template>
    <div class='tasks'>
        <h2>Tasks</h2>
        <ul>
            <li v-for='task in tasks'> 
                {{task.title}} - {{ task.description }} 
                <button v-on:click='deleteTask(task)'>x</button>
            </li>
        </ul>
    
        <form v-on:submit='addTask'>
        <!-- <form v-on:submit.prevent='addUser'> --> <!-- con esto prevenimo el comportamiento por defecto, la recarga de pagina al submit--> 
            <input type='text' v-model='newTask.title' placeHolder='title'>
            <input type='text' v-model='newTask.description' placeHolder='description'>
            <button type='submit'> Add </button>
        </form> <br>
    </div>
    </template>
    
    <script>
    import axios from 'axios'
    export default{    
        data(){ 
            return { 
                tasks: [],
                newTask: {},
                postURL: 'http://127.0.0.1:5050'
                
            }
        },
        methods:{
            addTask(e){            
                e.preventDefault(); // cancela el comportamiento por defecto, en este caso evitar que se vuelva a cargar la pagina luego del submit
                var config_request={
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }
                // with vue-resources            
                /*this.$http.post(this.postURL + '/create_task', this.newTask, config_request)
                    .then(res => {
                        this.tasks.push(res.body);                    
                    }, res => {
                        console.error(res.body);
                    });  */
    
                // with axios
                axios.post(this.postURL + '/create_task', this.newTask, { config_request })
                    .then(res => {                                         
                        this.tasks.push(res.data);
                        console.log(res.data)        ;
                    })
                    .catch((error) => {
                        console.log(error)
                    });
    
                this.newTask = {};
            },
            deleteTask(task){  
                var config_request={
                    'Content-Type': 'application/json',
                    'Access-Control-Allow-Origin': '*'
                }          
                axios.post(this.postURL + '/delete_task', {id:task.id}, { config_request })
                    .then(res => {                      
                        this.tasks.splice(this.tasks.indexOf(task), 1);
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
            //this.$http.post(this.postURL + '/tasks')
            //    .then(res => this.tasks = res.body);
            
            //with axios
            axios.post(this.postURL + '/tasks').then((res) => {  
                    console.log(res);
                    this.tasks = res.data;            
            })
            .catch((error) => {
                console.log(error)
            })
        }
    
    }
    </script>
    
    <style>
        .tasks{
            background-color: #cccccc;
        }
    </style>