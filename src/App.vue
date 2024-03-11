<script>
import axios from "axios";
import Pagination from './components/Pagination.vue';

export default {
    name: 'App',
    components: {
      Pagination
    },
    data() {
      return {
        pokemons: [],
        search: "",
        offset: 0,
        total: 151,
        limit: 12,
        count: 0
      };
    },
    created (){
        this.getResults();
    },
    methods: {
      getResults() {
        axios
        .get(`https://pokeapi.co/api/v2/pokemon?limit=12&offset=${this.count}`)
        .then((res) => {
                this.pokemons = res.data.results;
        })
        .catch((error) => {
                console.log(error);
        });
      },
      getId(pokemon) {
        return pokemon.url.split("/")[6]
      },
      changePage(value) {
        // debugger
        this.offset = value;
        this.count = value*12;
        this.getResults();
      }
    },
    computed: {
      filterPokemon() {
        return this.pokemons.filter((item) => {
          return item.name.includes(this.search);
        })
      },
    }
};

</script>

<template>
  <div class="container">
     <input v-model="search" placeholder="pesquisar">
       <div class="row">
        <div class="col" v-for="pokemon in filterPokemon" :key="pokemon.name">
          {{ getId(pokemon) }}
          <img 
            :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getId(pokemon)}.png`" 
            :alt="pokemon.name"
            width="80%"
          />
          <div class="card">
            <h2>{{ pokemon.name }}</h2>
          </div>
      </div>
    </div>
    <div id="pag">
      <pagination
        v-if="pokemons.length"
        :offset="offset"
        :total="total"
        :limit="limit"
        @change-page="changePage"
      />
    </div>
  </div>
</template>