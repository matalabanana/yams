<script>

import Joueur from './Joueur.vue'

export default {
  name: 'Partie',
  components: { Joueur }
}
</script>


<template>
	<div>
    Affichage ici de la partie Partie: {{ $route.params.id }}
    <hr> 
    <div style="float: left"> <Joueur prenom="Théo" libelle="true"> </Joueur> </div>
    <div style="float: left"> <Joueur prenom="Manon"> </Joueur> </div>
  </div>
  
</template> 
