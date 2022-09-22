<template>
  <div class="game-content">
    <section class="image-container">
      <img :src="gameDetails.background_image" />
    </section>
    <section class="details">
      <div class="flex-row space">
        </div>
      <div>
        <h3>
          {{gameDetails.name}}
        </h3>
        <p>{{gameDetails.description_raw}}</p>
          <router-link to="/">Back to Home</router-link>
      </div>
    </section>
  </div>
</template>

<script>
import axios from 'axios';
const API_KEY = process.env.VUE_APP_API_KEY
  export default {
    name: 'GameDetails',
    data: () => ({
      gameDetails: {}
    }),
    mounted: async function() {
      let gameId = this.$route.params.game_id
      await this.getGameDetails(gameId)
    },
    methods: {
      async getGameDetails(gameId) {
        console.log(gameId)
        let res = await axios.get(`https://api.rawg.io/api/games/${gameId}?key=${API_KEY}`)
        this.gameDetails = res.data
      }
    }
  }
</script>
