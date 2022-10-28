<template>
    <div class="heading" @click="titleClicked">{{text}}</div>
    <div class="card-item col-md-3 text-center" data-id={{internalId}}>
        <div class="card-box"><a class="remove-btn" href="#">X</a>
            <img class="photo" width="150px" src={{pokemonImage}} alt={{pokemonName}}/> 
            <h2 class="name mb-2" >{{pokemonName}}</h2>
            <ul class="type-list" >{{pokemonTypes}}</ul>
        </div>
    </div>'
</template>

<script>
    export default {
        name: 'PokemonCard',
        props: {
            pokemonData: {
                type: Object,
                required: true
            }
        },
        methods:{
            deletePokemon(id) {
                this.$emit('delete')
            }
        },
        data() {
            return {
             internalId: this.pokemonData.internalId,
             pokemonImage: this.pokemonData.sprites.front_default,
             pokemonName: this.pokemonData.name,
             pokemonTypes: ''
            }
        },
        created () {
            for (let i = 0; i < pokemonData.types.length; i++) {
                let pokemonType = pokemonData.types[i]
                this.pokemonTypes += '<li class="type-item">' + pokemonType.type.name + '</li>'
            }
        }
     }
    
</script>
<style scoped>
</style>
