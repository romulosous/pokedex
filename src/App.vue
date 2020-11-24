<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/images.png">
      <hr>
      <h4 class="is-size-4">Pokedex</h4>
      <input type="text" class="input is-rounded" placeholder="Buscar pokemon pelo nome" v-model="busca">
      <button class="button is-fullwidth is-success" id="buscaBtn" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"; //fazer requisições http dentro desse componente vue
import Pokemon from "./components/Pokemon";
export default {
  name: 'App',
  components: {
    Pokemon
  },
  data(){
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get('https://pokeapi.co/api/v2/pokemon?limit=151&offset=0').then(res => {
      console.log('pegou a lista de pokemons');
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  methods: {
    buscar(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' ' ){
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },
  computed: {
    // resultadoBusca(){
    //   if(this.busca == '' || this.busca == ' ' ){
    //     return this.pokemons;
    //   }else{
    //     return this.pokemons.filter(pokemon => pokemon.name == this.busca);
    //   }
    // }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#buscaBtn{
  margin-top: 2%;
}
</style>
