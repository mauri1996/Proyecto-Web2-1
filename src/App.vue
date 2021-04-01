<template>
    <div id="app" class="flex flex-col items-center justify-center h-screen">
      <img class="im" :src="Url" />
      <Title/>
      <Info />    
    </div>
</template>

<script>
import Info from './components/Info'
import Title from './components/Title'

export default {
  name: 'App',
  data(){
    return{
      Url:'https://img.freepik.com/free-vector/white-abstract-background_23-2148810113.jpg?size=626&ext=jpg&ga=GA1.2.1991903213.1617148800'
    }
  },
  components: {
    Info,
    Title
  }
}
</script>

<style>
.im {
  position: absolute;
  height: 100vh;
  width: 100%;
  z-index: -1;
  opacity: 0.5;
}
/* #app {
  z-index: 1;
} 
#app::before{
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  opacity: 0.2;  
  z-index: -1;
} */
</style>
