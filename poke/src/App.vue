
<template>

  <div class="cards">
    <!-- <div class="card"
  v-for="p in pokemon"  :key="p.id">
    <div class="title">
      {{p.name}}
    </div> -->
    <card v-for="pokemon in pokemons" 
    :key="pokemon.id" 
    @click="fetchEvolutions(pokemon)"
    :class="{ opace: pokemon.id !== selectedId}"
    class="card"
    >
    <template v-slot:title>
      {{pokemon.name}} #{{pokemon.id}}
  </template>

  <template v-slot:content>
    <img :src="pokemon.sprites"> 
  </template>

  <template v-slot:description>
     <div v-for="type in pokemon.types" :key="type">
            {{type}} 
            </div>
  </template>
    </card>
  </div>

  <div class="cards">
  <card v-for="pokemon in evolution" 
    :key="pokemon.id" 
    @click="fetchEvolutions(pokemon)"
    >
    <template v-slot:title>
      {{pokemon.name}} #{{pokemon.id}}
  </template>

  <template v-slot:content>
    <img :src="pokemon.sprites"> 
  </template>

  <template v-slot:description>
     <div v-for="type in pokemon.types" :key="type">
            {{type}} 
            </div>
  </template>
    </card>
  </div>


  
</template>

<script>
// import {ref} from 'vue';
import Card from './components/Card.vue';
const api = 'https://pokeapi.co/api/v2/pokemon'
const IDS = [1, 4, 7]
export default {
  components: {
    Card
  },
  // setup(){
  //   const pokemon = ref(0)
  //   const evolution = ref(0)
  //   const selectedId =null
  
  data() {
    return {
      pokemons: [],
      evolution: [],
      selectedId: null
    }
  },

  async created() {
    this.fetchData(IDS)
    this.pokemons = await this.fetchData(IDS)
  },


  methods: {
    async fetchEvolutions(pokemon){
this.evolution= await this.fetchData(
  [pokemon.id +1,pokemon.id +2])
  this.selectedId = pokemon.id
    },
    async fetchData(ids) {
    
      const responses = await Promise.all(
        ids.map(id => window.fetch(`${api}/${id}`))
      )
     
      const json = await Promise.all(
        responses.map(data => data.json())
      )
      // console.log(json)//array data


      return json.map(dataum => ({
        id: dataum.id,
        name: dataum.name,
        sprites: dataum.sprites.other['official-artwork'].front_default,
        types: dataum.types.map(type => type.type.name)
      }))
    
      //  console.log(this.pokemon)//proxy data comes 
    }
  }

}

</script>




<style>
img {
  width: 100%;
}
.cards{
  display: flex;
}
.opace {
  opacity: 0.5;
}
.card:hover {
opacity: 1.0;
}
</style>
