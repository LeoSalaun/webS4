<template>
    <input type="number" v-model="maxValue">
    {{ filteredData }}
    <div id="PokemonList" v-for="pokemon in this.filterData">
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
        computed: {
            filteredData() {
                // let data = [2, 4, 5]
                // const filterFunc = (item) => { return item < this.maxValue }
                // data = data.filter(filterFunc)
                // if (!this.pokemonList.results) return []
                // return this.pokemonList.results.slice(0, 4)
                if (!this.pokemonList.results) return []
                const filterName = (item) => { return item.name.includes(this.filterParameters.searchName) }
                let data = this.pokemonList.results.filter(filterName)
                return data
            }
        },
        data() {
            return {
                maxValue: 3,
                pokemonList: {},
                filterParameters: {
                    // searchName: {type: String, default: "null"},
                    // searchId: {type: Number, default: -1},
                    // searchType1: {type: String, default: "null"},
                    // searchType2: {type: String, default: "null"},
                    // searchGeneration: {type: Number, default: -1}
                }
            }
        },
        async created() {
            await this.retrievePokemonList()
        },
        // props: {
        //     searchName: {type: String, default: "null"},
        //     searchId: {type: Number, default: -1},
        //     searchType1: {type: String, default: "null"},
        //     searchType2: {type: String, default: "null"},
        //     searchGeneration: {type: Number, default: -1}
        // },
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
            },

            updateFilterParameters(filterData) {
                this.filterData = filterData
                console.log(this.filterData)
            },

            getPokemonById(searchId) {
                console.log(searchId)
            }
        },
        components: { PokemonHeader }
    }
</script>

<style>
    
</style>