<template>
    <HeaderVue/>
    <div class="container">
        <router-view/>
    </div>
</template>

<script>

import HeaderVue from './components/Header.vue'

export default {
  name: "App",
  components:{
    HeaderVue
  }
}
</script>



<style>

</style>
