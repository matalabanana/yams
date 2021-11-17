<script>

import De from './De.vue'

export default {
  name: 'Jeu',
  components: {
    De
  }, 
  data() {
    return {
      count:0, 	
      des: [  
        { id:0, valeur:0, relance: true },
        { id:1, valeur:0, relance: true },
        { id:2, valeur:0, relance: true },
        { id:3, valeur:0, relance: true },
        { id:4, valeur:0, relance: true }
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
    }
  }
}
</script>


<template>
	<div>
		<button v-on:click="clearJeu"> Nouveau </button>

		<button v-on:click="lancerDes">Lancer Dés</button>

		<hr>
		Mon jeu - nombre de lancer : {{count}} / 3
    <br> 
		<De v-for="de in des" :key="de.id" v-bind:id="de.id" v-bind:valeur="de.valeur" v-bind:relance="de.relance" @toggle-de="changeDeStatut"> </De> 
		
	</div>
</template> 
