<style lang="scss">

body {
	box-sizing: border-box;
	margin: 0; padding: 0;
	height: 100vh; width: 100vw;
}

::-webkit-scrollbar {
	width: 0;
}


#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  width: 100%;
  //background: url('/stars.jpg');
  background-size: cover;
}

#app a {
	color: #d7d;
}

</style>


<template lang="pug">

//TroisImp
LightExample

</template>


<script setup>
import TroisImp from './components/TroisImp.vue'
import LightExample from './components/LightExample.vue'
</script>



