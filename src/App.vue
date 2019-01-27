<template>
    <div id="app">
        <bar-nav></bar-nav>
        <router-view/>
        <my-footer></my-footer>
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


<style scoped>
</style>