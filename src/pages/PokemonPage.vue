<template>

    <h1 v-if="!pokemon">Cargando...</h1>

    <div v-else>
        <h1> ¿Quién es este pokémon? </h1>

        <PokemonPicture
          :pokemonId="pokemon.id"
          :showPokemon="showPokemon" 
        />

        <PokemonOptions 
            :pokemons="pokemonArr" 
            @selection="checkAnswer"
        />

        <div v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newQuestion"> 
                Siguiente  
            </button>
        </div>

    </div>

</template>


<script>

import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'

// console.log( getPokemonOptions() )


export default {
    components: { PokemonOptions, PokemonPicture },
    data() {
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {

        async mixPokemonArrays() {
            this.pokemonArr = await getPokemonOptions()
            
            const rndInt = Math.floor( Math.random() * 4 )
            this.pokemon = this.pokemonArr[ rndInt ]
        },

        checkAnswer( selectedId ) {
            this.showPokemon = true
            this.showAnswer  = true

            if( selectedId === this.pokemon.id ){
                this.message = `Correcto, ${ this.pokemon.name} `
            } else {
                this.message = `Oops, era ${ this.pokemon.name}`
            }
        },

        newQuestion() {
            this.showPokemon = false
            this.showAnswer  = false

            this.pokemon = null

            this.pokemonArr = []
            this.mixPokemonArrays()
        }

    },
    mounted() {
        this.mixPokemonArrays()
    }
}
</script>