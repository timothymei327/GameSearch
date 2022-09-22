<template>
<form class="sort">
  <div>Sort by Rating: </div>
  <select @change="switchSelect($event)" v-model="selected" class="dropdown">
    <option value="Select" disabled>Select</option>
    <option>Ascending</option>
    <option>Descending</option>
  </select>
</form>
  <div className="container-grid" v-for="game in games" :key="game.id">
    <GameCard :game="game" @click="selectGame(game.id)" />
  </div>

</template>

<script>
import axios from 'axios';
import GameCard from '../components/GameCard.vue';
const API_KEY = process.env.VUE_APP_API_KEY
  export default {
    name: 'ViewGames',
    components: { GameCard },
    data: () => ({
      games: []
    }),
    mounted: async function() {
      await this.getGamesByGenre()
      console.log(this.games)
    },
    methods: {
      async getGamesByGenre() {
        let genreId = this.$route.params.genre_id
        let res = await axios.get(`https://api.rawg.io/api/games?genres=${genreId}&key=${API_KEY}`)
        this.games = res.data.results
      },
      ascendingSort(){
        this.games.sort((a, b) => {
          return a.rating - b.rating
        })
      },
      descendingSort(){
        this.games.sort((a, b) => {
          return b.rating - a.rating
        })
      }, 
      switchSelect(e){
        this.selected = e.target.value
        if (this.selected === 'Ascending') {
          this.ascendingSort()
        } else if (this.selected === 'Descending') {
          this.descendingSort()
        }
      },
      selectGame(gameId) {
        this.$router.push(`/details/${gameId}`)
        console.log(gameId)
      }
    }
  }
</script>
