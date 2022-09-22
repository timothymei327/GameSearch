<template>
  <div class="card game-card">
    <div class="image-wrapper">
      <img :src="game.background_image">
    </div>
    <div class="info-wrapper flex-col">
      <h4>{{game.name}}</h4>
      <p>★ {{game.rating}}</p>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'GameCard',
    props: {
      game: {
        type: Object
      }
    }
  }
</script>
