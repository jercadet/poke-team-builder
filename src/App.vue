<script lang="ts">
import pokemonData from './assets/all_pokemon.json'

export default {
  data() {
    return {
      pokemonList: pokemonData.results,
      searchTerm: '',
      selectedPokemon: null
    }
  },
  computed: {
    filteredPokemonList() {
      return this.pokemonList.filter((pokemon) => pokemon.name.includes(this.searchTerm))
    }
  },
  methods: {
    async showPokemon(url: RequestInfo | URL) {
      const response = await fetch(url)
      if (!response.ok) {
        console.error(`Error fetching Pokemon: ${response.statusText}`)
        return
      }

      this.selectedPokemon = await response.json()
      console.log(this.selectedPokemon)
    }
  }
}
</script>

<template>
  <header>
    <!-- Logo made from Pokemon font generator from reddit user u/phoneticallySAARTHaK (https://pokemon-fonts-generator.netlify.app/) -->
    <!-- Logo size ratio is 6.149 -->
    <img
      alt="PokéTeam Builder Logo"
      class="logo"
      src="@/assets/poketeam-logo.png"
      width="768.617"
      height="125"
    />
  </header>

  <main>
    <div class="search-container">
      <input class="search-box" type="text" placeholder="Search Pokémon..." v-model="searchTerm" />
    </div>
    <div class="pokemon-details" v-if="selectedPokemon">
      <h2>{{ selectedPokemon.name }}</h2>
      <img :src="selectedPokemon.sprites.front_default" alt="selectedPokemon.name" />
      <p>Height: {{ selectedPokemon.height }}</p>
      <p>Weight: {{ selectedPokemon.weight }}</p>

      <div v-for="(stat, index) in selectedPokemon.stats" :key="index">
        <p>{{ stat.stat.name }}: {{ stat.base_stat }}</p>
      </div>
    </div>
    <ul>
      <li v-for="pokemon in filteredPokemonList" :key="pokemon.id" class="pokemon-item">
        <a href="#" @click="showPokemon(pokemon.url)">{{ pokemon.name }}</a>
      </li>
    </ul>
  </main>
</template>

<style scoped>
header {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.logo {
  margin: 0 0 0 0;
}

img {
  max-width: 100%;
  height: 100%;
  justify-content: center;
}

.search-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  gap: 10px;
}

.search-box {
  width: 30%;
  height: 50px;
  font-size: 1.5em;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  margin: 40px 0;
  text-align: center;
}

.pokemon-item {
  float: left;
  margin: 10px;
}
.pokemon-item a {
  color: #ffffff;
  text-decoration: none;
  font-size: 16px;
  transition: color 0.3s ease;
  text-transform: capitalize;
}

.pokemon-item a:hover {
  color: #3b4cca;
}

ul {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 5px;
  list-style: none;
}

.pokemon-details {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 30%;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #272727;
  border-radius: 10px;
  color: #ffffff;
  text-transform: capitalize;
  background-color: #00000066;
}

.pokemon-details img {
  width: 100px;
  height: 100px;
}
</style>
