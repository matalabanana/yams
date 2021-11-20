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
      lanceur:0, 
      message: "Bienvenue sur le jeu de Yam's", 
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
      var au_tour_de = this.lanceur % this.joueurs.length
      if (au_tour_de != payload.joueur) 
      {
        this.message = "C'est au tour du joueur "+(1+au_tour_de)+" de jouer ! "; 
      } else if (this.count == 0) {
        this.message = "Lancer les dés !"; 
      } else {
        var n = 0


        if (payload.figure < 6) { n = this.compte16(1+payload.figure); }
        else if (payload.figure== 6) { n = this.paireBrelan(2); } // Paire
        else if (payload.figure== 7) { n = this.paireBrelan(3); } // Brelan
        else if (payload.figure== 8) { n = this.paireBrelan(4); } // Carre
        else if (payload.figure== 9) { n = 25; } // Full
        else if (payload.figure==10) { n = 30; } // Petite Suite 
        else if (payload.figure==11) { n = 40; } // Grande Suite 
        else if (payload.figure==12) { n = this.paireBrelan(5); } // Yams 
        else if (payload.figure==13) { this.des.forEach(i => n+=i.valeur) } // Chance 


        console.log("Ecrit "+n+" pour la figure "+payload.figure+" du joueur "+payload.joueur); 
        this.$set(this.joueurs[payload.joueur].points, payload.figure, n); 
        this.count = 0; 

        this.des.forEach(i => {i.valeur=0; i.relance=true;}) 

        this.lanceur += 1; 
      }
    }, 
    paireBrelan(n) {
      var bonification = [0,0,7,30,40,50][n]  // bonification de 30 pour le Brelan, 50 pour le Yams 
      for (let i=6;i>0;i--) {
        if (this.compte16(i) >= n*i) { return bonification + n*i; }
      }
      return 0; 
    }, 
    compte16(x) {
      var n = 0
      this.des.forEach(i => i.valeur==x ? n += x : n)
      console.log("Compte les "+x+" et trouve "+n)
      return n; 
    },
    ajouteJoueur() {
      this.joueurs.push({points:[-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1,-1]})
    }, 
  }
}
</script>


<template>
	<div>

    <h1> Jeu {{ $route.params.id }} </h1> 

    <div class="alert alert-info" role="alert" > {{message}}  </div>


    <div v-if="joueurs.length > 0">
      Nb de lancer {{lanceur}}, c'est donc au joueur {{ 1+ lanceur % joueurs.length}}
      <table>
      <tr ref="cinqDes">
      <De v-for="de in des" :key="de.id" v-bind:id="de.id" v-bind:valeur="de.valeur" v-bind:relance="de.relance" @toggle-de="changeDeStatut"> </De> 
      </tr>
      </table> 

      <button class="btn btn-warning" v-on:click="lancerDes">Lancer {{count}} / 3</button>

    </div>


    <br> <br>


    <div class="col-lg-6"> 


      <br  style="margin-top:50px;"> 

      <button class="btn btn-success" @click="ajouteJoueur" v-if="lanceur==0"> Ajouter un joueur </button> 

      <table class="table" v-if="joueurs.length>0"> 
      <tr><th>  </th><th>  </th>
        <th title="">1</th> 
        <th title="">2</th> 
        <th title="">3</th> 
        <th title="">4</th> 
        <th title="">5</th> 
        <th title="">6</th> 
        <th title="Paire, 20 points">Paire</th> 
        <th title="Brelan, 30 points">Br.</th> 
        <th title="Carré, 40 points">Carré</th> 
        <th title="Full, 25 points">Full</th> 
        <th title="Petite suite, 30 points">P.S.</th> 
        <th title="Grande suite, 40 points">G.S.</th> 
        <th title="Yam's, 50 points">Yam's</th> 
        <th title="Total des 5 dés">Chance</th> 
        <th title="Bonus de +35 si +63 pts">Bonus 35</th> 
        <th title="">Total</th> 
      </tr> 
      <Joueur v-for="(j,idx) in joueurs" :key="idx" 
        v-bind:id="idx" 
        v-bind:marques="j.points" 
        v-bind:actif="(idx == lanceur % joueurs.length)"   
        @set-figure="setFigure"> </Joueur> 
      </table> 
    </div>





	</div>
</template> 
