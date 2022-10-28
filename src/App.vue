<template>
  <div>
    <div class="row">   
      <div class="col-md-12 text-center">
          <h1 class="title my-4">Pokédex</h1>
      </div>
    </div>
    <div class="row">   
            <form id="searchForm" class="form-inline col-md-8 offset-md-2">
                <div class="form-group mb-2 mr-2">
                    <input type="text" class="form-control" id="name" v-model="pokemonId" placeholder='Introduce el id o nombre del pokemon'>
                </div>
                <button type="submit" class="btn btn-secondary mb-2" @click="searchPokemon">Buscar</button>
            </form>
    </div>

  </div>
</template>

<script>
  import {fetch} from 'node-fetch';
  
  export default {
    name: "PokemonList",
    data(){
      return {
        pokemonId: "",
        pokemonList: []
      }
    },
    created () {
      console.info('-- POKEDEX LOADED --');
      
      this.pokemonList = JSON.parse(localStorage.getItem('pokemon_list'));

      if (this.pokemonList === null) {
        this.pokemonList = []
      }

    },
    methods: {
      searchPokemon() {
        if (this.pokemonId === '') {
          alert('Introduce un nombre o identificador de Pokemon')
        }

        if (this.pokemonId !== '') {
          this.getPokemon()
          this.pokemonId = ''
          // this.$refs.name.focus();
        }
      },
      getPokemon() {
        fetch(`https://pokeapi.co/api/v2/pokemon/${this.pokemonId}`)
          .then(res => res.json())
          .then(data => {
            console.log(data);
            let currentDate = new Date();
            data.internalId = currentDate.getTime();
            this.pokemonList.push(data);
            localStorage.setItem('pokemon_list', JSON.stringify(this.pokemonList));
          })
          .catch(err => {
            console.log(err);
            alert('no se ha encontrado el Pokemon: ',err.message());
          })
      },
      deletePokemon(id){
        let cardIndex = this.pokemonList.findIndex(function(item) {
          return item.internalId === id
        })

        if (cardIndex > -1) {
          this.pokemonList.splice(cardIndex, 1)
        }
        localStorage.setItem('pokemon_list', JSON.stringify(this.pokemonList))
      },
    },
  }
</script>

<style scoped>
</style>
