<script>

export default {
  name: 'Joueur',
  props: {
    id: { type: Number , required: true}, 
    marques: { type: Array, required: true}, 
    prenom: { type: String }
  }, 
  data() {
    return {
      editable: false, 
    }
  }, 
  methods: {
    lancerDes: function() {
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
    },
    emitSetFigure(id) {
      this.$emit('set-figure', { figure:id , joueur:this.id }); 
    },
    givefocus() {
      console.log('donne le focus'); 
      this.$refs.zonetxt.focus(); 
    }, 
    editePrenom() {
      this.editable = true; 
      this.marques.forEach(x => console.log(x));
    }
  }
}
</script>


<template>
  <tr> 
    <td class="text-end">{{id}}</td> 
    <td>
      <b v-show="!editable" @click="editePrenom">{{prenom}}</b> 
      <div v-show="editable">
        <input type="text" v-model="prenom" ref="zonetxt"> 
        <button class="btn btn-secondary" @click="editable=false"> ok </button> 
        <button class="btn btn-secondary" @click="givefocus"> focus </button> 
      </div>
    </td>
    <td v-for="(x, idx) in marques" :key="idx"> 
      {{x >= 0 ? x : ''}}  
      <div v-if="x<0"> <a @click="emitSetFigure(idx)">_</a> </div>
    </td>
    <td class="text-end"> 0 </td> 
  </tr> 
</template> 
