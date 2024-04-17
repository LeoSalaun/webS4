<template>
    <div id="Header">
        <div id="filterDiv">
            <form action="get" id="filters">
                <label for="searchBar">Search by Pokemon name: </label> <input v-model="this.searchName" v-on:input="submitName"  type="text" name="searchBar" id="searchBar"/>

                <label for="generation">Generation:</label> <input v-if="this.pokemonGenerations" v-model="this.searchGeneration" v-on:input="submitGeneration" type="number" id="generation" name="generation" min="1" :max="this.pokemonGenerations.length" />

                <label for="type1">First type:</label> <select v-if="this.typeList" v-model="this.searchType1" name="type1" id="type1">
                    <option v-for="pokemonType in this.typeList" :key="pokemonType.name" :value="pokemonType.name">{{ pokemonType.name }}</option>
                </select>

                <label for="type2">Second type:</label> <select v-if="this.typeList" v-model="this.searchType2" name="type2" id="type2">
                    <option v-for="pokemonType in this.typeList" :key="pokemonType.name" :value="pokemonType.name">{{ pokemonType.name }}</option>
                </select>
            </form> <br/>

            <button v-on:click="submitTypes">Get pokemons by types</button> 
        </div>

        <div id="resetSort">
            <button v-on:click="submitReset">Reset filters</button>

            <form action="get" id="sort">
                <button v-if="this.sortParameter == 'nameSort'" v-on:click="idSort">Sort by pokedex ID</button>
                <button v-if="this.sortParameter == 'idSort'" v-on:click="nameSort">Sort by pokemon name</button>
            </form>
        </div>
    </div>
</template>

<script>

    export default {
        name: 'Header',
        components: {},
        data() {
            return {
                pokemonTypes: {},
                pokemonGenerations: {},
                searchName: "",
                searchType1: "unknown",
                searchType2: "unknown",
                searchGeneration: 0,
                sortParameter: "idSort"
            }
        },
        async created() {
            await this.retrievePokemonTypes()
            await this.retrievePokemonGenerations()
        },
        mounted() {
            if(localStorage.searchName) this.searchName = localStorage.searchName;
            if(localStorage.searchType1) this.searchType1 = localStorage.searchType1;
            if(localStorage.searchType2) this.searchType2 = localStorage.searchType2;
            if(localStorage.searchGeneration) this.searchGeneration = localStorage.searchGeneration;
            if(localStorage.sortParameter) this.sortParameter = localStorage.sortParameter;
        },
        watch:{
            searchName(searchName) {
                localStorage.searchName = searchName;
            },
            searchType1(searchType1) {
                localStorage.searchType1 = searchType1;
            },
            searchType2(searchType2) {
                localStorage.searchType2 = searchType2;
            },
            searchGeneration(searchGeneration) {
                localStorage.searchGeneration = searchGeneration;
            }
        },
        computed: {
            typeList() {
                if (!this.pokemonTypes) return []
                else return this.pokemonTypes
            },
            generationList() {
                if (!this.pokemonGenerations) return []
                else return this.pokemonGenerations
            }
        },
        methods: {
            async getPokemonTypes() {
                const response = await fetch("https://pokeapi.co/api/v2/type")
                if (response.status == 200) {
                    const data = await response.json()
                    return data.results
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonTypes() {
                this.pokemonTypes = await this.getPokemonTypes()
            },
            
            async getPokemonGenerations() {
                const response = await fetch("https://pokeapi.co/api/v2/generation")
                if (response.status == 200) {
                    const data = await response.json()
                    return data.results
                } else {
                    new Error(response.statusText)
                }
            },

            async retrievePokemonGenerations() {
                this.pokemonGenerations = await this.getPokemonGenerations()
            },

            submitName() {
                this.$emit('submit-name', this.searchName)
            },

            submitTypes() {
                this.$emit('submit-types', [this.searchType1, this.searchType2])
            },

            submitGeneration() {
                if (this.pokemonGenerations[this.searchGeneration-1]) this.$emit('submit-generation', this.pokemonGenerations[this.searchGeneration-1].name)
                else this.$emit('submit-generation', "none")
            },

            submitReset() {
                this.searchName = ""
                this.searchType1 = "unknown"
                this.searchType2 = "unknown"
                this.searchGeneration = 0
                this.submitName()
                this.submitTypes()
                this.submitGeneration()
            },

            submitSort() {
                this.$emit('submit-sort', this.sortParameter)
            },

            idSort() {
                this.sortParameter = "idSort"
                localStorage.sortParameter = "idSort";
                this.submitSort()
            },

            nameSort() {
                this.sortParameter = "nameSort"
                localStorage.sortParameter = "nameSort";
                this.submitSort()
            }
        },
        emits: ['submit-name', 'submit-types', 'submit-generation', 'submit-sort']
    }
</script>

<style>
    #filters {
        display: flex;
        flex-direction: row;
    }
    
    #filters > label {
        margin-right: 0.25em;
    }

    input, select {
        margin-right: 1em;
    }

    #filterDiv {
        display: flex;
        flex-direction: row;
    }

    @media (max-width: 768px) {
        #filters {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr 1fr;
        }

        #filterDiv {
            flex-direction: column;
        }

        #resetSort {
            display: grid;
            grid-template-columns: 1fr 1fr;
            align-items: end;
        }

        #resetSort > button {
            height: 87%;
        }

        #sort > button {
            width: 100%;
            height: 100%;
        }

        #searchBar {
            width: 85%;
        }
    }
</style>