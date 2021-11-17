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
	<div class="deCarre" v-bind:class="{keep : !relance}" @click="emitToggleDe"> 
		{{valeur>0 ? valeur : '&nbsp;'}}  
	</div>
</template> 




<style>
	.keep { background:green; }
	.deCarre { font-size: 40px; cursor:pointer; border: 1px solid grey; padding: 5px; margin: 5px; float: left; }
</style> 