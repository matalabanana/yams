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
    <Joueur prenom="Gérard"> </Joueur>
  </div>
  
</template> 
