<template>
    <div id="app">
        <v-app id="inspire">
            <v-footer dark height="auto">
                <v-card flat tile class="indigo lighten-1 white--text text-xs-center">
                    <v-card-text>
                        <v-btn v-for="icon in icons" :key="icon" class="mx-3 white--text" icon>
                            <v-icon size="24px">{{ icon }}</v-icon> &nbsp Code in Github
                        </v-btn>
                    </v-card-text>
                    <v-card-text class="white--text pt-0">
                        Proyecto desarrollado en VueJs. Nunca olvide sus tareas con esta aplicacion, aqui podra anotar
                        todas sus tareas y filtrarlas por importancia, una vez completadas puede borrarlas! Tambien puede
                        filtrarlas por su importancia y cambiarles la prioridad si usted lo desea.

                        Codigo en Github ---
                    </v-card-text>
                    <v-divider></v-divider>
                    <v-card-text class="white--text">
                        &copy;2019 — <strong> {{nameDev}} </strong>
                    </v-card-text>
                </v-card>
            </v-footer>
        </v-app>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                nameDev : 'Francisco Javier Sánchez de la Torre'
            }
        }
    }
</script>

<style scoped>

</style>