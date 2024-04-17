<template>
    <Header v-on:submit-name="getPokemonsByName"
            v-on:submit-types="getPokemonsByTypes"
            v-on:submit-generation="getPokemonsByGeneration"
            v-on:submit-sort="sortPokemonList" /> <br/>
    <div v-if="this.pokemonList">
        <div id="page1" class="page">
            <button v-on:click="previousPage"><< Previous</button>
            <p>Page {{ this.page }}</p>
            <button v-on:click="nextPage">Next >></button>
        </div> <br/>

        <div id="PokemonList" class="grid">
            <PokemonCard v-for="pokemon in this.filteredData" :key="pokemon.name" :pokemonName="pokemon.name" :searchType1="this.searchType1" :searchType2="this.searchType2" class="pokemonCard"/>
        </div>
        
        <div id="page2" class="page">
            <button v-on:click="previousPage"><< Previous</button>
            <p>Page {{ this.page }}</p>
            <button v-on:click="nextPage">Next >></button>
        </div>
    </div>
</template>

<script>
    import PokemonCard from "./PokemonCard.vue"
    import Header from "./Header.vue"

    export default {
        name: 'PokemonList',
        computed: {
            filteredData() {
                if (!this.pokemonList) return []
                const filterName = (item) => {
                    return item.name.includes(this.searchName)
                }
                
                let data = this.pokemonList
                if (this.searchName != "") {
                    data = data.filter(filterName)
                }

                return (this.sortData(data)).slice((this.page-1)*20,this.page*20)
            }
        },
        data() {
            return {
                pokemonList: [],
                filterParameters: {},
                page: 1,
                searchName: "",
                searchType1: "unknown",
                searchType2: "unknown",
                searchGeneration: 0,
                sortParameter: "idSort"
            }
        },
        async created() {
            await this.retrievePokemonList()
        },

        methods: { 
            async getPokemonList() {
                const response = await fetch("https://pokeapi.co/api/v2/pokemon/?offset=0&limit=1025")
                if (response.status == 200) {
                    const data = await response.json()
                    return data.results
                } else {
                    new Error(response.statusText)
                }
            },

            async getPokemonListByGeneration() {
                const response = await fetch("https://pokeapi.co/api/v2/generation/"+this.searchGeneration)
                if (response.status == 200) {
                    const data = await response.json()
                    return data.pokemon_species
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonList() {
                this.pokemonList = await this.getPokemonList()
            },

            async retrievePokemonListByGeneration() {
                this.pokemonList = await this.getPokemonListByGeneration()
            },

            async updateGenerationFilter() {
                if (this.searchGeneration != "none") await this.retrievePokemonListByGeneration()
                else await this.retrievePokemonList()
            },

            previousPage() { if (this.page > 1) this.page-- },

            nextPage() {  this.page++ },

            sortData(data) {
                console.log(this.sortParameter)
                if (this.sortParameter == "idSort") return data.sort((a,b) => {
                    let idA = parseInt(a.url.substring(34, a.url.length-1))
                    let idB = parseInt(b.url.substring(34, b.url.length-1))
                    return idA - idB
                })
                else return data.sort((a,b) => a.name.localeCompare(b.name))
            },

            getPokemonsByName(searchName) {
                this.page = 1
                this.searchName = searchName
            },

            getPokemonsByTypes(searchTypes) {
                this.page = 1
                this.searchType1 = searchTypes[0]
                this.searchType2 = searchTypes[1]
            },

            async getPokemonsByGeneration(searchGeneration) {
                this.page = 1
                this.searchGeneration = searchGeneration
                await this.updateGenerationFilter()
            },

            sortPokemonList(sortParameter) {
                this.page = 1
                this.sortParameter = sortParameter
            }
        },
        components: { PokemonCard, Header }
    }
</script>

<style>
    .grid {
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
    }

    .page {
        display: flex;
        flex-direction: row;
        
    }

    .page > p {
        margin: 0px 1em;
    }

    @media (max-width: 768px) {
        .grid {
            grid-template-columns: 1fr 1fr;
        }
    }

    @media (max-width: 375px) {
        .grid {
            grid-template-columns: 1fr;
        }
    }
</style>