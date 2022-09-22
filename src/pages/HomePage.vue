<template>
  <div>
    <div class="search">
      <form  @submit="getSearchResults">
        <input placeholder="Search" @input="handleChange"/>
        <button type="submit">Submit</button>
      </form>
      <h2>Search Results</h2>
      <section class="search-results container-grid" v-for="game in searchResults" :key="game.id">
        <GameCard :game="game" @click="selectGame(game.id)"/>
      </section>
    </div>
    <div class="genres" v-if="!searched">
      <h2>Genres</h2>
      <section class="container-grid" v-for="genre in genres" :key="genre.id">
        <GenreCard :genre="genre" @click="selectGenre(genre.id)"/>
      </section>
    </div>
  </div>
</template>

<script>
import GenreCard from '../components/GenreCard.vue'
import GameCard from '../components/GameCard.vue'
import axios from 'axios'
const API_KEY = process.env.VUE_APP_API_KEY
  export default {
    name: 'HomePage',
    components: { GenreCard, GameCard },
    data: () => ({
      genres: [],
      searchQuery: '',
      searchResults: [],
      searched: false
    }),
    mounted: async function() {
      await this.getGenres()
    },
    methods: {
      async getGenres() {
        let res = await axios.get(`https://api.rawg.io/api/genres?key=${API_KEY}`)
        this.genres = res.data.results
      },
      async getSearchResults(e) {
        e.preventDefault()
        let res = await axios.get(`https://api.rawg.io/api/games?key=${API_KEY}&search=${this.searchQuery}`)
        this.searchResults = res.data.results
        this.searched = true
      },
      handleChange(e) {
        this.searchQuery = e.target.value
        console.log(event)
      },
      selectGame(gameId) {
        this.$router.push(`/details/${gameId}`)
        console.log(gameId)
      },
      selectGenre(genreId) {
        this.$router.push(`/games/${genreId}`)
        console.log(genreId)
      }
    }
  }
</script>