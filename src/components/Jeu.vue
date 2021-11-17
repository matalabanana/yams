<script>

import De from './De.vue'
import Joueur from './Joueur.vue'

export default {
  name: 'Jeu',
  components: {
    De, Joueur
  }, 
  data() {
    return {
      count:0,
      message: "Bienvenue !", 
      des: [  
        { id:0, valeur:0, relance: true },
        { id:1, valeur:0, relance: true },
        { id:2, valeur:0, relance: true },
        { id:3, valeur:0, relance: true },
        { id:4, valeur:0, relance: true }
      ], 
      joueurs: [
        { id:20, prenom:"Théo", libelle: true },
        { id:21, prenom:"Arthur", libelle: false } 
      ]}
  }, 
  methods: {
    lancerDes() {
      console.log('relance les dés éventuels... lecture du statut actuel et modif éventuelle'); 
      this.count++;
      for (let i=0; i < 5; i++) {
        if (this.des[i].relance) {
          this.des[i].valeur = 1 + Math.floor(Math.random() * 6); 
        }
      }
    }, 
    changeDeStatut(payload) {
      console.log('change le statut de '+payload.idex)
      if (this.des[payload.idex].valeur == 0) {
        this.des[payload.idex].relance = true;
      } else {
        this.des[payload.idex].relance = ! this.des[payload.idex].relance
      }
    },
    clearJeu() {
      for (let i=0; i < 5; i++) {
        this.des[i].valeur = 0
        this.des[i].relance = true; 
      }
    },
    setFigure(payload) {
      var x = [] 
      for (let i=0; i < 5; i++) {
        x.push(this.des[i].valeur)
      }
      this.message = "Le joueur "+payload.joueur+" choisit la figure "+payload.figure_description+" pour les dés ..."+x.join('.') 
    }
  }
}
</script>


<template>
	<div>

    <h1> Jeu {{ $route.params.id }} </h1> 

		<De v-for="de in des" :key="de.id" v-bind:id="de.id" v-bind:valeur="de.valeur" v-bind:relance="de.relance" @toggle-de="changeDeStatut"> </De> 

    <button v-on:click="lancerDes">Lancer {{count}} / 3</button>
		

    <hr style="clear:both;"> 
    <div> {{message}} </div> 
    <br><br>
    <Joueur v-for="j in joueurs" :key="j.id" v-bind:id="j.id" v-bind:prenom="j.prenom" v-bind:libelle="j.libelle" @set-figure="setFigure"> </Joueur> 



	</div>
</template> 
