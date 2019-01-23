<template>
  <div id="app">
    <bar-nav></bar-nav>
    <router-view/>

    

  </div>
</template>

<script>
  import BarNav from '@/components/BarNav'

  export default {
    name:'app',
    components:{
      BarNav
    }
  }
</script>


<style>
#app {
}
</style>