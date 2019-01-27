<template>
    <div id="app">
        <bar-nav></bar-nav>
        <router-view/>
    </div>
</template>

<script>
    import BarNav from '@/components/BarNav';
    import MyFooter from '@/components/MyFooter';
    export default {
        name: 'app',
        components: {
            BarNav,
            MyFooter
        }
    }
</script>


<style>
    #app {
    }
</style>