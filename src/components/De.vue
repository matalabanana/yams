<script>

export default {
  name: 'De',
  props: {
    id: { type: Number , required: true}, 
    valeur: { type: Number, default: 4 }, 
    relance: { type: Boolean, default: true }, 
  }, 
  methods: {
    emitToggleDe() {
      this.$emit('toggle-de', { idex: this.id }); 
      // nom de l'event, payload optionnel pour passer les donnees à l'écouteur 
    }
  }
}
</script>


<template>
	<div v-bind:class="{actif : relance}"> 
		Dé {{valeur>0 ? valeur : '?'}}  
		<button @click="emitToggleDe"> {{relance ? 'relance' : 'garde'}} </button> 
	</div>
</template> 




<style>
	.actif { color:red; }
</style> 