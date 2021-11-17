<script>

export default {
  name: 'Joueur',
  props: {
    prenom:  { type: String , required: true}, 
    libelle: { type: Boolean, default: false}, 
  }, 
  data() {
    return {
      resultats: [  
        { id: 0, pt:-1, description:'1'},  
        { id: 1, pt:-1, description:'2'},  
        { id: 2, pt:-1, description:'3'},  
        { id: 3, pt:-1, description:'4'},  
        { id: 4, pt:-1, description:'5'},  
        { id: 5, pt:-1, description:'6'},  
        { id: 6, pt:-1, description:'Brelan'},        // somme des 3 dés 
        { id: 7, pt:-1, description:'Carré'},         // somme des 4 dés 
        { id: 8, pt:-1, description:'Full'},          // 25 points
        { id: 9, pt:-1, description:'Petite suite'},  // 30 points
        { id:10, pt:-1, description:'Grande suite'},  // 40 points
        { id:11, pt:-1, description:'Yams'},          // 50 points
        { id:12, pt:-1, description:'Chance'},        // somme des 5 dés
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
      this.des[payload.idex].relance = ! this.des[payload.idex].relance
    }
  }
}
</script>


<template>
	<div>
		<b>{{prenom}}</b>
		<table style="border:1px dashed;margin-right:20px;"> 
			<tr v-for="x in resultats" :key="x.id">
				<td> <label v-if="libelle">{{x.description }} </label></td>
          <td style="min-width: 50px;"> {{x.pt >= 0 ? x.pt : '&nbsp;'}} </td>
			</tr>
		</table> 

	</div>
</template> 
