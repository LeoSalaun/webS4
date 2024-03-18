<template>
    <div v-if="this.exist" id="Pokemon">
        <p>Name: {{ this.pokemonData.name }}</p>
        <!--<p>Type 1: {{ this.pokemonData.types }}</p>
        <p>Type 2: {{ type2 }}</p>-->
        <div id="PokemonList" v-for="pokemonType in this.pokemonData.types">
            <p>Type {{ pokemonType.slot }}: {{ pokemonType.type.name }}</p>
        </div>
        <img :src="this.pokemonData.sprites.front_default" alt="" id="sprite" wigth="400px" height="400px">
    </div>
</template>

<script>
    export default {
        name: 'PokemonHeader',
        components: {},
        data() {
            return {
                pokemonData: {},
                exist: false
            }
        },
        async created() {
            await this.retrievePokemonData()
        },
        props: {
            url: {type: String, default: "https://raw.githubusercontent.com/PokeAPI/pokemon/201-question.png"}
        },
        methods: {
            async getPokemonById(pokemonId) {
                const response = await fetch("https://pokeapi.co/api/v2/pokemon/"+pokemonId)
                if (response.status == 200) {
                    const data = await response.json()
                    return data
                } else {
                    new Error(response.statusText)
                }
            },

            async getPokemonByUrl(pokemonUrl) {
                const response = await fetch(pokemonUrl)
                if (response.status == 200) {
                    const data = await response.json()
                    return data
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonData() {
                this.pokemonData = await this.getPokemonByUrl(this.url)
                this.exist = true
            }
        }
    }
</script>

<style>
    img {
        image-rendering: pixelated;
        image-rendering: -moz-crisp-edges;
        image-rendering: crisp-edges;
    }
</style>
  