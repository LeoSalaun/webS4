<template>
    <div v-if="this.exist">
        <div v-if="this.hasGoodTypes">
            <p>Name: {{ this.pokemonData.name }}</p>
            <!--<p>Type 1: {{ this.pokemonData.types }}</p>
            <p>Type 2: {{ type2 }}</p>-->
            <div class="pokemonTypes">
                <p>Type : </p>
                <p v-for="pokemonType in this.pokemonData.types" :key="pokemonType.type.name">{{ pokemonType.type.name }}</p>
            </div>
            <img v-if="this.pokemonData.sprites" :src="this.pokemonData.sprites.front_default" alt="" id="sprite">
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PokemonCard',
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
        deactivated() {
            this.$destroy();
        },
        computed: {
            hasGoodTypes() {
                // let filter = (this.pokemonData.types[0].name == this.searchType1.name || this.pokemonData.types[0].name == this.searchType2.name)
                    // && ((this.pokemonData.types.length == 1 && (this.searchType2 == {"default":"null"} || this.searchType1 == {"default":"null"}))
                    //  || (this.pokemonData.types.length == 2 && (this.pokemonData.types[1].name == this.searchType2.name
                    //                                          || this.pokemonData.types[1].name == this.searchType1.name)))
                //  if (!(filter) && this.pokemonData.types.length == 2) filter = (this.pokemonData.types[1].name == this.searchType1.name || this.pokemonData.types[1].name == this.searchType2.name)
                // console.log(this.pokemonData.types)
                // console.log(this.searchType1)
                // console.log(this.searchType2)
                //return filter

                //console.log(searchType1)

                if (!this.exist) return true

                if (this.searchType1 == "unknown" && this.searchType2 == "unknown") return true
                else if (this.searchType2 == "unknown") return this.hasType(this.searchType1)
                else if (this.searchType1 == "unknown") return this.hasType(this.searchType2)
                return (this.hasType(this.searchType1) && this.hasType(this.searchType2))

                // return true
            }
        },
        props: {
            pokemonName: "201",
            searchType1: "unknown",
            searchType2: "unknown"
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

            async getPokemonByUrl() {
                const response = await fetch("https://pokeapi.co/api/v2/pokemon/"+this.pokemonName)
                if (response.status == 200) {
                    const data = await response.json()
                    return data
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonData() {
                this.pokemonData = await this.getPokemonByUrl()
                this.exist = true
            },

            hasType(type) {
                return (this.pokemonData.types[0].type.name == type || (this.pokemonData.types.length > 1 && this.pokemonData.types[1].type.name == type))
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

    .pokemonTypes {
        display: flex;
        flex-direction: row;
    }

    .pokemonTypes > p {
        margin-right: 0.25em;
    }
</style>
  