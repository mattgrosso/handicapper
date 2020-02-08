<template>
  <div class="game-details">
    <div class="close" @click="$emit('close')">
      <i class="far fa-window-close"></i>
    </div>
    <div class="game-title-wrapper">
      <div class="image-wrapper">
        <img :src="game.thumbnail" :alt="game.title">
      </div>
      <div class="game-info-wrapper">
        <h1>{{game.title}}</h1>
        <h3>Average Score: {{gameStats.averageScore}}</h3>
      </div>
    </div>
    <ul class="player-list">
      <li>
        <div class="header-row player-row">
          <p class="name">Name</p>
          <p class="number">Plays</p>
          <p class="wins">Wins</p>
          <p class="average">Avg</p>
          <p class="handicap">+/-</p>
        </div>
      </li>
      <li v-for="player in sortedPlayers" :key="player.name">
        <player-tile :player="player" :handicap="handicapFor(player)"/>
      </li>
    </ul>
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
      sortedPlayers () {
        const playersArray = Object.keys(this.players).map((player) => {
          return this.players[player];
        });

        const sortedArray = playersArray.sort((firstEl, secondEl) => {
          if (this.handicapFor(firstEl) > this.handicapFor(secondEl)) {
            return 1;
          } else if (this.handicapFor(firstEl) < this.handicapFor(secondEl)) {
            return -1;
          } else {
            return 0;
          }
        });

        return sortedArray;
      }
    },
    methods: {
      handicapFor (player) {
        return Math.round(this.gameStats.averageScore - player.average);
      }
    }
  };

  export default component;
</script>

<style lang="scss">
  .game-details {
    background-color: white;
    border-radius: 5px;
    margin: 12px;
    padding: 12px;
    position: relative;

    .close {
      cursor: pointer;
      font-size: 1.5rem;
      padding: 12px;
      position: absolute;
      right: 0;
      top: 0;
    }

    .game-title-wrapper {
      display: flex;

      .image-wrapper {
        align-items: center;
        display: flex;
        max-width: 40%;

        img {
          width: 100%;
        }
      }

      .game-info-wrapper {
        max-width: 60%;
        padding: 12px;

        h1 {
          font-size: 1.5rem;
          margin-bottom: 12px;
        }

        h3 {
          font-size: 1rem;
        }
      }
    }

    .player-list {
      margin: 12px 0;

      li {
        background-color: #94c393;
        width: 100%;

        &:nth-child(2n) {
          background-color: #ceeccc;
        }

        .player-row {
          align-items: stretch;
          display: flex;
          padding: 6px 4px;

          p {
            align-items: center;
            box-sizing: border-box;
            display: flex;
            justify-content: center;
            padding: 4px;
            width: 18%;

            &.name {
              justify-content: flex-start;
              width: 28%
            }
          }

          &.header-row {
            background-color: white;
            border-bottom: 1px solid black;
            font-weight: bold;
          }
        }
      }
    }
  }
</style>
