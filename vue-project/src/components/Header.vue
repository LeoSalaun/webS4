<template>
    <div id="Header">
        <form action="get" id="filters">
            <label for="searchBar">Search by Pokemon name: </label> <input v-model="searchName" type="text" name="searchBar" id="searchBar"/> <br/>
            <label for="type1">First type:</label> <select v-model="searchType1" name="type1" id="type1"></select> <br/>
            <label for="type2">Second type:</label> <select v-model="searchType2" name="type2" id="type2"></select> <br/>
            <label for="generation">Generation:</label> <input v-model="searchGeneration" type="number" id="generation" name="generation" min="1" max="9" /> <br/>
        </form>
        <button v-on:click="submit">Search</button>

        <form action="get" id="searchId">
            <label for="pokedexId">Pokedex ID:</label> <input v-model="searchID" type="number" id="pokedexId" name="pokedexId" min="1" /> <br/>
        </form>
        <button v-on:click="submitId">Find Pokemon with pokedex ID</button>

        <form action="get" id="sort">
            <input type="radio" id="pokedexIdSort" name="drone" value="huey" checked />
            <label for="huey">Huey</label> <br/>
        </form>
        <button v-on:click="submitId">Find Pokemon with pokedex ID</button>
    </div>
</template>

<script>

    export default {
        name: 'Header',
        components: {},
        data() {
            return {
                searchName: "",
                searchID: -1,
                searchType1: "",
                searchType2: "",
                searchGeneration: "",
                pokemonTypes: {},
                pokemonGenerations: {}
            }
        },
        props: {},
        methods: {
            submit() {
                this.$emit('submit', {
                    searchName: this.searchName,
                    searchType1: this.searchType1,
                    searchType2: this.searchType2,
                    searchGeneration: this.searchGeneration
                })
            },

            submitId() {
                this.$emit('submit-id', this.searchID)
}
        },
        emits: ['submit', 'submit-id']
    }
</script>

<style>

</style>