<template>
    <div class="row" id="fondo">
        <div class="col-lg-5 col-centered text-white text-center">
            Desarrollado por Francisco Javier Sánchez de la Torre<br>
            Código disponible en <a href="https://github.com/Franklonchas/VueNotes" id="enlaceGit"><img
                src="../assets/github-logo.png">&nbsp;GitHub</a>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                nameDev: 'Francisco Javier Sánchez de la Torre'
            }
        }
    }
</script>

<style scoped>
    #fondo {
        background: #222222;
    }

    #enlaceGit {
        color: #00bc8c;
    }

    .col-centered {
        float: none;
        margin: 0 auto;
    }
</style>