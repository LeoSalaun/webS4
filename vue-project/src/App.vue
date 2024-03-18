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
  <Header/>
  Hello World
  <PokemonList/>
</template>

<script>
  import Pokemon from "./components/Pokemon.vue"
  import Header from "./components/Header.vue"
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
    components : { Pokemon, Header, PokemonList },
    created() {
      console.log(20);
    },
    data() {
      return {
        id: pokemonId,
        sprite: pokemon.sprites.front_default,
        name: pokemon.name,
        type1: pokemon.types[0].type.name,
        type2: pokemonType2,
        url: "https://pokeapi.co/api/v2/pokemon/1/"
      }
    }
  }
</script>

<style>
  
</style>
