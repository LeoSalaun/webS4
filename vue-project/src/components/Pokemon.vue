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
    import Moves from "./Moves.vue"

    export default {
        name: 'Pokemon',
        components: {
            Moves
        },
        data() {
            return {
                pokemonId: this.id,
                pokemonName: this.name,
                pokemonType1: this.type1,
                pokemonType2: this.type2,
                pokemonData: {},
                exist: false
            }
        },
        async created() {
            await this.retrievePokemonData()
        },
        props: {
            id: {Number, default: 201},
            sprite: {type: String, default: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/201-question.png"},
            name: {type: String, default: "Unknown"},
            type1: {type: String, default: "Psychic"},
            type2: {type: String, default: "None"},
            url: {type: String, default: "None"}
        },
        methods: {
            async getPokemonById(pokemonId) {
                const response = await fetch("https://pokeapi.co/api/v2/pokemon/"+pokemonId)
                if (response.status == 200) {
                    const pokeData = await response.json()
                    return pokeData
                } else {
                    new Error(response.statusText)
                }
            },

            async getPokemonByUrl(pokemonUrl) {
                const response = await fetch(pokemonUrl)
                if (response.status == 200) {
                    const pokeData = await response.json()
                    return pokeData
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
  