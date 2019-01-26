<template>
    <div class="container">
        <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <div class="input-group-text">
                    Priority defect Low
                </div>
            </div>
            <input type="text" class="form-control" @keyup.enter="addNewTodo()" v-model="newTodoText" id="new-todo"
                   placeholder="Put your reminder">
        </div>

        <div class="input-group mb-3">
            <div class="input-group-prepend">
                <button v-on:click="findTask" class="btn btn-outline-secondary" type="button" id="button-addon1">Find a
                    task
                </button>
            </div>
            <input v-model="task" type="text" class="form-control" placeholder="Write here to find a task..."
                   aria-label="Example text with button addon" aria-describedby="button-addon1">
        </div>

        Hay {{items.length}} tareas en tu lista de tareas.
        Hay {{ completadas }} tareas completadas en tu lista de tareas.
        <a href="#" v-on:click="deleteCompletedTasks">x Borrar tareas completadas</a>

        <transition-group name="custom-classes-transition" enter-active-class="animated tada"
                          leave-active-class="animated bounceOutRight">
            <div v-for="(item, index) in items" v-bind:key="item" class="card">
                <div class="card-body">
                    <h5 v-if="item.completada==false" class="card-footer">
                        <img src="../assets/unchecked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;Reminder:
                        {{item.tarea}}
                    </h5>
                    <h5 v-else class="card-footer">
                        <img src="../assets/checked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;Reminder:
                        {{item.tarea}}
                    </h5>
                    <p class="card-text">
                        <button v-if="item.prioridad == 'Low'" type="button" class="btn btn-info btn-sm">Low</button>
                        <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToLow(item)">
                            Low
                        </button>
                        <button v-if="item.prioridad == 'Medium'" type="button" class="btn btn-warning btn-sm">Medium
                        </button>
                        <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToMedium(item)">
                            Medium
                        </button>
                        <button v-if="item.prioridad == 'High'" type="button" class="btn btn-danger btn-sm">High
                        </button>
                        <button v-else type="button" class="btn btn-secondary btn-sm" v-on:click="changeToHigh(item)">
                            High
                        </button>
                    </p>
                    <p class="card-text"> Creation Date: {{item.fecha}}</p>
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
                items: [],
                auxItems: []
            }
        },
        methods: {
            addNewTodo: function () {
                let nuevaTarea = {
                    tarea: this.newTodoText,
                    prioridad: "Low",
                    fecha: new Date().toLocaleString(),
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
            },
            changeToLow: function (item) {
                item.prioridad = 'Low';
                this.filterOrder();
            },
            changeToMedium: function (item) {
                item.prioridad = 'Medium';
                this.filterOrder();
            },
            changeToHigh: function (item) {
                item.prioridad = 'High';
                this.filterOrder();
            },
            changeToComplete: function (item) {
                if (item.completada == true) {
                    item.completada = false;
                    localStorage.setItem('todos', JSON.stringify(this.items));
                } else
                    item.completada = true;
                localStorage.setItem('todos', JSON.stringify(this.items));

            },
            filterOrder: function () {
                this.auxItems = [];
                for (let i = 0; i < this.items.length; i++) {
                    if (this.items[i].prioridad === "High") {
                        this.auxItems.push(this.items[i]);
                    }
                }
                for (let i = 0; i < this.items.length; i++) {
                    if (this.items[i].prioridad === "Medium") {
                        this.auxItems.push(this.items[i]);
                    }
                }
                for (let i = 0; i < this.items.length; i++) {
                    if (this.items[i].prioridad === "Low") {
                        this.auxItems.push(this.items[i]);
                    }
                }
                this.items = this.auxItems;
                localStorage.setItem('todos', JSON.stringify(this.items));
            },
            findTask: function () {
                for (let i = 0; i < this.items.length; i++) {
                    if (this.items[i].tarea === this.task) {
                        console.log("Encontrada");
                    }
                }
            },
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