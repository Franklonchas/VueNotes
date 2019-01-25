<template>
    <div class="container">
        <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <div class="input-group-text">
                    <input type="checkbox" aria-label="Checkbox for following text input">{{Dificultad[0]}}
                    <input type="checkbox" aria-label="Checkbox for following text input">{{Dificultad[1]}}
                    <input type="checkbox" aria-label="Checkbox for following text input">{{Dificultad[2]}}
                </div>
            </div>
            <input type="text" class="form-control" @keyup.enter="addNewTodo()" v-model="newTodoText" id="new-todo"
                   placeholder="Put your reminder">
        </div>

        Hay {{items.length}} tareas en tu lista de tareas.
        Hay {{ completadas }} tareas completadas en tu lista de tareas.
        <a href="#" v-on:click="deleteCompletedTasks">x Borrar tareas completadas</a>

        <transition-group name="custom-classes-transition" enter-active-class="animated tada"
                    leave-active-class="animated bounceOutRight">
            <div v-for="(item, index) in items" v-bind:key="item" class="card">
                <div class="card-body">
                    <h5 class="card-title">Reminder: {{item.tarea}}&nbsp;&nbsp;&nbsp;</h5>
                    <h6 class="card-footer">Priority: {{item.prioridad}}</h6>
                    <p class="card-text">Date of creation{{fecha_creacion}}</p>
                    <a href="#" class="btn btn-danger" v-on:click="deleteNote(index)">Delete</a>
                </div>
            </div>
        </transition-group>


    </div>

</template>

<script>
    export default {
        name: "GestionNotas",
        data: function () {
            return {
                newTodoText: '',
                Dificultad: ['Low', 'Medium', 'High'],
                items: []
            }
        },
        methods: {
            addNewTodo: function () {
                let nuevaTarea = {
                    tarea: this.newTodoText,
                    prioridad: "Low",
                    fecha_creacion: new Date().getDate(),
                    completada: false
                };
                this.items.push(nuevaTarea);
                this.newTodoText = '';
                localStorage.setItem('todos', JSON.stringify(this.items))
            },
            deleteNote: function (index) {
                this.items.splice(index, 1);
                localStorage.setItem('todos', JSON.stringify(this.items));
            },
            deleteCompletedTasks: function () {
                for (let i = this.items.length - 1; i => 0; i--) {
                    if (this.items[i].completada == true) {
                        this.items.splice(i, 1);
                        localStorage.setItem('todos', JSON.stringify(this.items));
                    }
                }
            }
        },
        changeToComplete: function(task){
            task.status = true;
        },
        computed: {
            completadas: function () {
                let list;
                list = this.items.filter(function (item) {
                    return item.completada;
                });
                return list.length;
            }
        },
        mounted() {
            var items = JSON.parse(localStorage.getItem('todos'));
            if (items) {
                this.items = items;
            }
        },
    }
</script>

<style scoped>

</style>