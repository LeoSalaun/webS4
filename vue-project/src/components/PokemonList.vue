<template>
    <div id="PokemonList" v-for="pokemon in this.pokemonList.results">
        <!--<PokemonHeader v-bind:url="pokemon.url"/>-->
        <PokemonHeader v-bind:url="pokemon.url"/>
    </div>
</template>

<script>
    //import PokemonHeader from "./components/PokemonHeader.vue"
    import PokemonHeader from "./PokemonHeader.vue"

    export default {
        name: 'PokemonList',
        components: {},
        data() {
            return {
                pokemonList: {}
            }
        },
        async created() {
            await this.retrievePokemonList()
        },
        props: {
            searchName: {type: String, default: "null"},
            searchId: {type: Number, default: -1},
            searchType1: {type: String, default: "null"},
            searchType2: {type: String, default: "null"},
            searchGeneration: {type: Number, default: -1}
        },
        methods: {
            async getPokemonList() {
                const response = await fetch("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=20")
                if (response.status == 200) {
                    const data = await response.json()
                    return data
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonList() {
                this.pokemonList = await this.getPokemonList()
                console.log(this.pokemonList)
            }
        },
        components: { PokemonHeader }
    }
</script>

<style>
    
</style>