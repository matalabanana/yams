<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    
    <h1> <HelloWorld msg="Bienvenue sur le jeu de Yam's !"/> </h1> 

    Les deux dossiers src/componentset src/viewscontiennent des composants Vue.
    <br> 
    La principale différence est que certains composants Vue agissent comme des vues pour le routage. 

  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script>
