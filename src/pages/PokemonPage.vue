<template>
<div>
<h1 v-if="!pokemon">Espere por favor...</h1>
<div v-else>
    <h1>    QUIEN ES ESE POKEMON?</h1>
    <PokemonPictureVue :pokemonId="pokemon.id" :showPokemon="showPokemon"></PokemonPictureVue>
    <PokemonOptionsVue :pokemons="pokemonArr" @selection="checkAnswer"></PokemonOptionsVue>
    <template v-if="showAnswer">
    <h2>{{message}}</h2>
    <button @click="newGame">Nuevo juego</button>
    </template>
</div>
</div>
</template>

<script>

import PokemonOptionsVue from '@/components/PokemonOptions.vue'
import PokemonPictureVue from '@/components/PokemonPicture.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions.js'

console.log(getPokemonOptions())
export default {
    components:{
        PokemonOptionsVue,PokemonPictureVue
    },
    data(){
        return{
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',

        }
    },
    methods:{
        async mixPokemonsArray(){
            this.pokemonArr = await getPokemonOptions()
            const randInt = Math.floor(Math.random()* 4 )
            this.pokemon = this.pokemonArr[randInt]
        },
        checkAnswer(pokemonId){
            this.showPokemon=true
            if(this.pokemon.id == pokemonId){
                this.message = `Correcto, ${this.pokemon.name}`
            }
            else{
                this.message = `Oops, era ${this.pokemon.name}`                
            }
            this.showAnswer = true
        },
        newGame(){
            this.pokemon= null
            this.showPokemon= false
            this.showAnswer= false
            this.mixPokemonsArray()
        }
    },
    mounted(){
        this.mixPokemonsArray()
    }

}
</script>

<style>

</style>