<!-- <script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style> -->






<template>
  <header>
    <h1>The Pok'I'Dex</h1>
    <img src="https://sweezy-cursors.com/wp-content/uploads/cursor/pokemon-pokeball-pixel-animated/pokemon-pokeball-pixel-animated-custom-cursor.gif" alt="" width="200px" height="100px">
    <p>(the 'I' stands for 'IMAC'...)</p>
  </header>
  <br/>
  <PokemonList/>
  <br/>
  <footer>
    <p>Made by Léo SALAUN</p>
  </footer>
</template>

<script>
  import Pokemon from "./components/Pokemon.vue"
  import PokemonList from "./components/PokemonList.vue"

  

  async function getPokemonById(pokemonId) {
        const response = await fetch("https://pokeapi.co/api/v2/pokemon/"+pokemonId)
        if (response.status == 200) {
            const data = await response.json()
            return data
        } else {
            new Error(response.statusText)
        }
    }

    async function getPokemonByUrl(pokemonUrl) {
        const response = await fetch(pokemonUrl)
        if (response.status == 200) {
            const data = await response.json()
            return data
        } else {
            new Error(response.statusText)
        }
    }

    let pokemonId = Math.floor(Math.random() * 1302)
    if (pokemonId > 1025) {
      pokemonId += 8975
    }

    let pokemonUrl = "https://pokeapi.co/api/v2/pokemon/"+pokemonId+"/"

    let pokemon = await getPokemonByUrl(pokemonUrl)

    let pokemonType2
    if (pokemon.types.length == 1) {
      pokemonType2 = "None"
    }
    else {
      pokemonType2 = pokemon.types[1].type.name
    }
    console.log(pokemonType2)

  export default {
    name: 'App',
    components : { Pokemon, PokemonList },
    created() {
      console.log(20);
    },
    data() {
      return {
        searchName: {type: String, default: ""},
        searchId: {type: Number, default: -1},
        searchType1: {type: String, default: "null"},
        searchType2: {type: String, default: "null"},
        searchGeneration: {type: String, default: "null"},
        sortParameter: {type: String, default: "null"}
      }
    }
  }
</script>

<style>
  #app {
    margin: 2em;
    width: 100%;
    grid-template-columns: 1fr;
  }

  header {
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  footer {
    margin-top: 2em;
  }

  h1 {
    font-size: 5em;
    margin-right: 0.25em;
  }
</style>
