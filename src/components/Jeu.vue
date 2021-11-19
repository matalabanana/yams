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
      editionPrenom: false, 
      des: [  
        { id:0, valeur:0, relance: true },
        { id:1, valeur:0, relance: true },
        { id:2, valeur:0, relance: true },
        { id:3, valeur:0, relance: true },
        { id:4, valeur:0, relance: true }
      ], 
      joueurs: [] 
    }
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
    }, 
    editPrenom(payload) {
      this.editionPrenom = true; 
      console.log('modification prénom '+payload.joueur); 
      this.joueurs.forEach( function(j) {
        if (j.id==payload.joueur) {
          j.prenom = "Gustave"; 
        }
      }); 
    }, 
    ajouteJoueur() {
      this.joueurs.push({})
    }
  }
}
</script>


<template>
	<div>

    <h1> Jeu {{ $route.params.id }} </h1> 

    <table>
    <tr>
		<De v-for="de in des" :key="de.id" v-bind:id="de.id" v-bind:valeur="de.valeur" v-bind:relance="de.relance" @toggle-de="changeDeStatut"> </De> 
    </tr>
    </table> 

    <button class="btn btn-warning" v-on:click="lancerDes">Lancer {{count}} / 3</button>
    <br> <br>
    <button class="btn btn-success" @click="ajouteJoueur"> Ajouter un joueur </button> 

    <div v-show="editionPrenom">
      <input placeholder="Nom du joueur"> 
      <button class="btn btn-success"> ok </button> 
    </div>
		
    <table class="table">
    <tr><th>Joueur </th>
      <th title="">1</th> 
      <th title="">2</th> 
      <th title="">3</th> 
      <th title="">4</th> 
      <th title="">5</th> 
      <th title="">6</th> 
      <th title="Bonus de +35 si +63 pts">Bonus ?</th> 
      <th title="Paire, 20 points">Paire</th> 
      <th title="Brelan, 30 points">Br.</th> 
      <th title="Carré, 40 points">Carré</th> 
      <th title="Full, 25 points">Full</th> 
      <th title="Petite suite, 30 points">P.S.</th> 
      <th title="Grande suite, 40 points">G.S.</th> 
      <th title="Yam's, 50 points">Yam's</th> 
      <th title="Total des 5 dés">Chance</th> 
      <th title="">Total</th> 
    </tr> 
    <Joueur v-for="(j,idx) in joueurs" :key="idx" v-bind:prenom="j.prenom" v-bind:libelle="j.libelle" 
      @edit-prenom="editPrenom"
      @set-figure="setFigure"> </Joueur> 
    </table> 

	</div>
</template> 
