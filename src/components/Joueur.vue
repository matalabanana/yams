<script>

export default {
  name: 'Joueur',
  props: {
    id: { type: Number , required: true}, 
    actif: { type: Boolean },
    marques: { type: Array, required: true}, 
  }, 
  data() {
    return {
      editable: false, 
      prenom: (this.id==0 ? 'Jeanne' : 'Marie'), 
    }
  }, 
  computed: {
    bonus() {
      var n = 0; 
      this.marques.slice(0, 6).forEach( i => n += Math.max(0,i)) 
      if (n >= 63) { return 35; }
      return 0; 
    }, 
    total() {
      var n = this.bonus; 
      this.marques.forEach(i => n += Math.max(0,i)) 
      return n; 
    }
  }, 
  methods: {
    emitSetFigure(id) {
      console.log('Emet évènement set-figure'); 
      this.$emit('set-figure', { figure:id , joueur:this.id }); 
    },
    givefocus() {
      console.log('donne le focus'); 
      this.$refs.zonetxt.focus(); 
    }, 
    editePrenom() {
      this.editable = true; 
    }
  }
}
</script>


<template>
  <tr v-bind:class="{'text-primary': actif}"> 
    <td class="text-end"> {{1+id}}</td> 
    <td>
      <b v-show="!editable" @click="editePrenom">{{prenom}}</b> 
      <div v-show="editable">
        <input type="text" v-model="prenom" ref="zonetxt"> 
        <button class="btn btn-secondary" @click="editable=false"> ok </button> 
        <button class="btn btn-secondary" @click="givefocus"> focus </button> 
      </div>
    </td>
    <td class="text-end" v-for="(v,k) in marques" :key="k"> 
      <span v-if="v>=0">{{v}}</span>
      <a @click="emitSetFigure(k)" v-if="v<0">_</a> 
    </td>
    <td class="text-end"> {{bonus}} </td> 
    <td class="text-end"> {{total}} </td> 
  </tr> 
</template> 
