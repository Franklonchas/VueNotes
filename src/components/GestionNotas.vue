<template>
    <div class="container">
        <div class="container text-white" id="name">
            Proyecto Vue.js - Francisco Javier Sánchez de la Torre
        </div>
        <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
        <div class="container">
            <input type="text" class="form-control" @keyup.enter="addNewTodo()" v-model="newTodoText" id="new-todo"
                   placeholder="¿Qué quieres recordar?">
        </div>

        <div class="container">
            <input v-model="search" type="text" class="form-control"
                   placeholder="Escribe aquí para buscar una nota..."
                   aria-label="Example text with button addon" aria-describedby="button-addon1">
        </div>
        <div class="container text-white">
            <hr style="width:75%; border-color:grey;">

            <img src="../assets/notes.png">&nbsp;Hay {{items.length}} tareas en tu lista de tareas. || Hay {{
            completadas
            }} tareas completadas en tu lista de tareas. ||
            <a href="#" v-on:click="deleteCompletedTasks" style="color: #f39c12;">&nbsp;x Borrar tareas completadas</a>
            <hr style="width:75%; border-color:grey;">
        </div>

        <div v-if="findTask == ''" v-bind:key="1">
            <transition-group name="custom-classes-transition" enter-active-class="animated tada"
                              leave-active-class="animated bounceOutRight">
                <div v-for="(item, index) in items" v-bind:key="item" class="card">
                    <div class="card-body text-white bg-dark">
                        <h5 v-if="item.completada===false" class="card-footer">
                            <img src="../assets/unchecked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;{{item.tarea}}
                        </h5>
                        <h5 v-else class="card-footer" style="color:#00bc8c; text-decoration:line-through;">
                            <img src="../assets/checked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;{{item.tarea}}
                        </h5>
                        <p class="card-text">
                            <button v-if="item.prioridad === 'Low'" type="button" class="btn btn-info btn-sm">Low
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToLow(item)">
                                Low
                            </button>
                            &nbsp;
                            <button v-if="item.prioridad === 'Medium'" type="button" class="btn btn-warning btn-sm">
                                Medium
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToMedium(item)">
                                Medium
                            </button>
                            &nbsp;
                            <button v-if="item.prioridad === 'High'" type="button" class="btn btn-danger btn-sm">High
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToHigh(item)">
                                High
                            </button>
                        </p>
                        <p class="card-text"> Fecha de Creación: {{item.fecha}}<img src="../assets/delete.png"
                                                                                    class="delete"
                                                                                    v-on:click="deleteNote(index)"
                                                                                    height="42" width="42"></p>
                    </div>
                </div>
            </transition-group>
        </div>

        <div v-if="findTask != ''" v-bind:key="2">
            <transition-group name="custom-classes-transition" enter-active-class="animated tada"
                              leave-active-class="animated bounceOutRight">
                <div v-for="(item, index) in findTask" v-bind:key="item" class="card">
                    <div class="card-body text-white bg-dark">
                        <h5 v-if="item.completada===false" class="card-footer">
                            <img src="../assets/unchecked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;{{item.tarea}}
                        </h5>
                        <h5 v-else class="card-footer" style="color:#00bc8c; text-decoration:line-through;">
                            <img src="../assets/checked.png" v-on:click="changeToComplete(item)">&nbsp;&nbsp;&nbsp;{{item.tarea}}
                        </h5>
                        <p class="card-text">
                            <button v-if="item.prioridad === 'Low'" type="button" class="btn btn-info btn-sm">Low
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToLow(item)">
                                Low
                            </button>
                            &nbsp;
                            <button v-if="item.prioridad === 'Medium'" type="button" class="btn btn-warning btn-sm">
                                Medium
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToMedium(item)">
                                Medium
                            </button>
                            &nbsp;
                            <button v-if="item.prioridad === 'High'" type="button" class="btn btn-danger btn-sm">High
                            </button>
                            <button v-else type="button" class="btn btn-secondary btn-sm"
                                    v-on:click="changeToHigh(item)">
                                High
                            </button>
                        </p>
                        <p class="card-text"> Fecha de Creación: {{item.fecha}}<img src="../assets/delete.png"
                                                                                    class="delete"
                                                                                    v-on:click="deleteNote(index)"
                                                                                    height="42" width="42"></p>
                    </div>
                </div>
            </transition-group>
        </div>

        <br>
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
                auxItems: [],
                search: ''
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
            startInterval: function () {
                this.updateInterval = setInterval(() => {
                    this.currentDate = new Date();
                    this.fecha = this.currentDate - this.fecha;
                }, 60 * 1000);
            },
        },
        computed: {
            completadas: function () {
                let list;
                list = this.items.filter(function (item) {
                    return item.completada;
                });
                return list.length;
            },
            findTask: function () {
                if (this.search === '' || this.search === ' ') {
                    return false;
                } else {
                    return this.items.filter(item => {
                        return item.tarea.toLowerCase().indexOf(this.search.toLowerCase()) > -1;
                    })
                }
            },
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
    #name {
        font-size: 30px;
    }

    .delete {
        margin-left: 790px;
    }
</style>