<template>
  <div class="game-details">
    <div class="close" @click="$emit('close')">
      Close
    </div>
    <div class="game-title-wrapper">
      <h1>{{game.title}}</h1>
      <h3>Average Score: {{gameStats.averageScore}}</h3>
    </div>
    <div class="player-tiles-wrapper" v-for="player in players" :key="player.name">
      <player-tile :player="player" :game-average="gameStats.averageScore"/>
    </div>
  </div>
</template>

<script>
  import PlayerTile from "./PlayerTile.vue";

  const component = {
    props: {
      game: Object,
      players: Object,
      gameStats: Object
    },
    components: {
      PlayerTile
    },
    computed: {
      playerNames () {
        if (this.players) {
          return Object.keys(this.players)
        } else {
          console.error("players is not defined");
        }
      }
    }
  };

  export default component;
</script>

<style lang="scss" scoped>
  .game-details {
    .close {
      cursor: pointer;
      position: absolute;
      right: 32px;
      top: 32px;
      border: 1px solid black;
      padding: 12px;

      &:hover {
        background: black;
        color: white;
      }
    }
  }
</style>
