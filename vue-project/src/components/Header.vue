<template>
    <div id="Header">
        <form action="get">
            <label for="searchBar">Search by Pokemon name: </label> <input v-on:input="changeName" type="text" name="searchBar" id="searchBar"/> <br/>
            <label for="pokedexId">Search by Pokedex ID:</label> <input v-on:input="changeID" type="number" id="pokedexId" name="pokedexId" min="1" /> <br/>
            <label for="type1">First type:</label> <select v-on:input="changeType1" name="type1" id="type1"></select> <br/>
            <label for="type2">Second type:</label> <select v-on:input="changeType2" name="type2" id="type2"></select> <br/>
            <label for="generation">Generation:</label> <input v-on:input="changeGeneration" type="number" id="generation" name="generation" min="1" max="9" /> <br/>
            <input type="submit" value="Search" />
        </form>
        <button v-on:click="sayHello">Yo</button>
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
            sayHello() {
                console.log(this)
            },

            changeName() {
                this.searchName = document.getElementById("searchBar").value
            },

            changeID() {
                this.searchID = document.getElementById("pokedexID").value
            },

            changeType1() {
                this.searchType1 = document.getElementById("type1").value
            },

            changeType2() {
                this.searchType2 = document.getElementById("type2").value
            },

            changeGeneration() {
                this.searchGeneration = document.getElementById("generation").value
            }
        }
    }
</script>

<style>

</style>