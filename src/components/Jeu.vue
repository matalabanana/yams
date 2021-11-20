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
      lamain:0, // joueur qui lance 
      message: '', 
      des: [  
        { id:0, valeur:0, relance: true },
        { id:1, valeur:0, relance: true },
        { id:2, valeur:0, relance: true },
        { id:3, valeur:0, relance: true },
        { id:4, valeur:0, relance: true }
      ], 
      joueurs: [
        { prenom: 'Sarah', points:[-1] }
      ] 
    }
  }, 
  methods: {
    lancerDes() {
      console.log('relance les dés éventuels... lecture du statut actuel et modif éventuelle'); 
      if (this.count<3) {
        for (let i=0; i < 5; i++) {
          if (this.des[i].relance) {
            this.des[i].valeur = 1 + Math.floor(Math.random() * 6); 
          }
        }
        this.count++;
      }
    }, 
    changeDeStatut(payload) {
      console.log('change le statut de '+payload.idex)
      if (this.des[payload.idex].valeur > 0) {
        this.des[payload.idex].relance = ! this.des[payload.idex].relance
      }
    },
    setFigure(payload) {
      var x = 0 
      for (let i=0; i < 5; i++) {
        x = x + this.des[i].valeur
      }
      this.message = "Attribue le choix "+payload.figure+" au joueur "+payload.joueur 
      this.joueurs[payload.joueur].points.push(x); 
      this.count = 0; 
      for (let i=0; i < 5; i++) {
        this.des[i].valeur = 0
        this.des[i].relance = true; 
      }
    }, 
    ajouteJoueur() {
      this.joueurs.push({prenom: 'John', points:[-1]})
    }
  }
}
</script>


<template>
	<div>

    <h1> Jeu {{ $route.params.id }} </h1> 

    <table>
    <tr ref="cinqDes">
		<De v-for="de in des" :key="de.id" v-bind:id="de.id" v-bind:valeur="de.valeur" v-bind:relance="de.relance" @toggle-de="changeDeStatut"> </De> 
    </tr>
    </table> 

    <button class="btn btn-warning" v-on:click="lancerDes">Lancer {{count}} / 3</button>
    <br> <br>
    <button class="btn btn-success" @click="ajouteJoueur"> Ajouter un joueur </button> 
		
    <table class="table" v-if="joueurs.length>0" style="margin-top:50px;"> 
    <tr><th>  </th><th>  </th>
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
    <Joueur v-for="(j,idx) in joueurs" :key="idx" 
      v-bind:id="idx" 
      v-bind:marques="j.points" 
      v-bind:prenom="j.prenom" 
      @set-figure="setFigure"> </Joueur> 
    </table> 

    <div> {{message}} </div>




	</div>
</template> 
