<template>
  <div class="player-tile">
    <h2>{{player.name}}</h2>
    <h3>Plays: {{player.number}}</h3>
    <h3>Wins: {{player.wins}}</h3>
    <h3>Average Score: {{player.average}}</h3>
    <h3>Handicap: {{handicap}}</h3>
  </div>
</template>

<script>
  const component = {
    props: {
      player: Object,
      gameAverage: Number
    },
    computed: {
      handicap () {
        return Math.round(this.gameAverage - this.player.average);
      }
    }
  };

  export default component;
</script>

<style lang="scss" scoped>
  .player-tile {
    display: flex;
    align-items: center;

    h2,
    h3 {
      margin: 12px;
    }
  }
</style>
