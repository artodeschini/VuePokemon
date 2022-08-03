<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <img src="./assets/logo_pokemon.png" alt="logo pokemon" />
      <h4 class="is size-1">Pokedex Sample with Vue</h4>
      <input class="input is-rounded" type="text" placeholder="Buscar Pokemon pelo Nome" v-model="filtro" />
      <button id="btn_busca" class="button is-medium is-warning" @click="buscar">Buscar</button>
      <div v-for="(poke, index) in pokemonsFiltrados" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index + 1" />
      </div>
    </div>
  </div>  
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';

export default {
  name: 'App',
  data() {
    return {
      filtro: '',
      pokemons: [],
      pokemonsFiltrados: []
    }
  },
  created: function() {
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      console.log("pegando a lista de pokemons");
      this.pokemons = res.data.results;
      // console.log(this.pokemons);
      this.pokemonsFiltrados = this.pokemons;

    });
  },
  computed: {
      // resultadoBusca: function() {
      //   if (this.busca == '' || this.busca == ' ') {
      //     return this.pokemons;
      //   }
      //   return this.pokemons.filter(pokemon => pokemon.nome == this.busca);
      // }
    }, 
  components: {
    Pokemon
  },
  methods: {
    buscar: function() {
      this.pokemonsFiltrados = this.pokemons;
      //console.log('em buscar');
      //console.log(this.pokemons);

      if (this.filtro == '' || this.filtro == ' ') {
        this.pokemonsFiltrados = this.pokemons;
      } else {
        //console.log('else de buscar');
        //console.log(this.filtro);
        var temp = this.filtro.toLowerCase();
        this.pokemonsFiltrados = this.pokemons.filter(function (p) {
          console.log(p.name);
          return p.name.toLowerCase().indexOf(temp) != -1;
        });
      }
    }
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

#btn_busca {
  margin-top: 2%;
}

</style>
