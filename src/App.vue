<template>
  <div class="geral">
  <navBar/>
  <corpo/>

  </div>
</template>

<script>
import navBar from './components/navBar.vue';
import corpo from './components/corpo.vue'

export default {
  name: 'App',
  components:{
    navBar,
    corpo
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  overflow-x: hidden;
  background-color: #2a2333;
}
body{
  width: 100%;
  height: 100%;

}


</style>
