<template>
  <pokemon-cards :pokemons="pokemons" @chosen="fetchEvolutions" :selectedId="selectedId" />

  <pokemon-cards :pokemons="evolution" />
  <div class="cards">
    <card v-for="pokemon in evolution" :key="pokemon.id" @click="fetchEvolutions(pokemon)">
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
import { ref, created } from 'vue';

//  import Card from './components/Card.vue';
import PokemonCards from './components/PokemonCards.vue';
const api = 'https://pokeapi.co/api/v2/pokemon'
const IDS = [1, 4, 7]
export default {
  components: {
    PokemonCards,

  },
  async setup() {
    const pokemons = ref(0)
    const evolution = ref(0)
    const selectedId = ref(0)

     const fetchEvolutions = async(pokemon) => {
      evolution.value = await fetchData(
        [pokemon.id + 1, pokemon.id + 2])
      selectedId.value = pokemon.id
    },
     created(async () => {
        fetchData(IDS)
        pokemons.value = await fetchData(IDS)
      })
    fetchData = (ids) => {

      const responses = await Promise.all(
        ids.map(id => window.fetch(`${api}/${id}`))
      )

      const json = await Promise.all(
        responses.map(data => data.json())
      )
      return json.map(dataum => ({
        id: dataum.id,
        name: dataum.name,
        sprites: dataum.sprites.other['official-artwork'].front_default,
        types: dataum.types.map(type => type.type.name)
      }))
    }

    return {
      pokemons,
      evolution,
      selectedId
    }


  }

}
</script>

<style>

</style>