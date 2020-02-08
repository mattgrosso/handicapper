<template>
  <div id="app">
    <div
      v-if="!detailedGame"
      class="game-tiles">
      <div class="search">
        <input
          v-model="search"
          type="text"
          placeholder="search...">
        <div class="clear-search" @click="clearSearch" v-if="search">
          <i class="far fa-times-circle"></i>
        </div>
      </div>
      <game-tile
        v-for="game in filteredGames"
        :key="game.id"
        :game="game"
        @selected="processGame(game)"/>
    </div>
    <game-details
      v-if="detailedGame"
      :game="detailedGame"
      :players="players"
      :game-stats="gameStats"
      @close="detailedGame = null"/>
  </div>
</template>

<script>
import axios from "axios";
import GameTile from "./components/GameTile.vue";
import GameDetails from "./components/GameDetails.vue";

export default {
  name: "App",
  components: {
    GameTile,
    GameDetails
  },
  data () {
    return {
      games: null,
      detailedGame: null,
      players: null,
      gameStats: null,
      search: ""
    };
  },
  computed: {
    filteredGames () {
      if (this.games) {
        return this.games.filter((game) => {
          if (this.search) {
            return game.title.toLowerCase().includes(this.search.toLowerCase());
          } else {
            return true;
          }
        })
      } else {
        return this.games;
      }
    }
  },
  mounted () {
    axios
      .get(`https://www.boardgamegeek.com/xmlapi2/collection?username=mattgrosso&minplays=1&own=1&subtype=boardgame`)
      .then((response) => {
        const xmlObject = this.xmlStringToObject(response.data);
        this.games = this.cleanGames(this.xmlToJson(xmlObject).items.item);
      })
  },
  methods: {
    getPlaysForGame (gameID) {
      return axios
        .get(`https://www.boardgamegeek.com/xmlapi2/plays?username=mattgrosso&id=${gameID}`)
        .then((response) => {
          const xmlObject = this.xmlStringToObject(response.data);
          return this.xmlToJson(xmlObject);
        })
    },
    xmlStringToObject (xmlString) {
      return ( new window.DOMParser() ).parseFromString(xmlString, "text/xml");
    },
    xmlToJson(xml) {
      var obj = {};

      if (xml.nodeType == 1) {
        if (xml.attributes.length > 0) {
          obj["attributes"] = {};

          for (var j = 0; j < xml.attributes.length; j++) {
            var attribute = xml.attributes.item(j);
            obj["attributes"][attribute.nodeName] = attribute.nodeValue;
          }
        }
      } else if (xml.nodeType == 3) {
        obj = xml.nodeValue;
      }

      if (xml.hasChildNodes()) {
        for(var i = 0; i < xml.childNodes.length; i++) {
          var item = xml.childNodes.item(i);
          var nodeName = item.nodeName;

          if (typeof(obj[nodeName]) == "undefined") {
            obj[nodeName] = this.xmlToJson(item);
          } else {
            if (typeof(obj[nodeName].push) == "undefined") {
              var old = obj[nodeName];
              obj[nodeName] = [];
              obj[nodeName].push(old);
            }

            obj[nodeName].push(this.xmlToJson(item));
          }
        }
      }
      return obj;
    },
    cleanGames (games) {
      return games.map((game) => {
        return {
          id: game.attributes.objectid,
          title: game.name["#text"],
          imageURL: game.image["#text"],
          thumbnail: game.thumbnail["#text"],
          plays: {
            number: game.numplays["#text"]
          }
        }
      })
    },
    cleanPlayers (plays) {
      if (!Array.isArray(plays)) {
        plays = [plays];
      }

      let players = {};

      plays.forEach((eachPlay) => {
        if (!Array.isArray(eachPlay.players.player)) {
          eachPlay.players.player = [eachPlay.players.player];
        }

        eachPlay.players.player.forEach((eachPlayer) => {
          const target = eachPlayer.attributes;
          if (!players[target.name]) {
            players[target.name] = {
              name: target.name,
              number: 1,
              wins: parseInt(target.win),
              handicap: 0,
              scores: [parseInt(target.score)],
              average: parseInt(target.score) || 0
            };
          } else {
            players[target.name].number++;
            players[target.name].wins = players[target.name].wins + parseInt(target.win);
            players[target.name].scores.push(parseInt(target.score));

            const averageScore = Math.round(players[target.name].scores.reduce((acc, current) => acc + current ) / players[target.name].scores.length);

            players[target.name].average = averageScore || 0;
          }
        })
      })

      return players;
    },
    getGameStats (plays) {
      if (!Array.isArray(plays)) {
        plays = [plays];
      }

      const scores = [];

      plays.forEach((play) => {
        play.players.player.forEach((player) => {
          scores.push(parseInt(player.attributes.score));
        })
      })

      const averageScore = Math.round(scores.reduce((acc, current) => acc + current ) / scores.length)

      return {
        name: plays[0].item.attributes.name,
        totalPlays: plays.length,
        averageScore: averageScore || 0
      }
    },
    processGame (game) {
      this.getPlaysForGame(game.id).then((response) => {
        this.players = this.cleanPlayers(response.plays.play);
        this.gameStats = this.getGameStats(response.plays.play)
        this.detailedGame = game;
      })
    },
    clearSearch () {
      this.search = "";
    }
  }
}
</script>

<style lang="scss">
  @import "./assets/styles/reset.scss";
  @import "./assets/styles/variables.scss";

  body {
    background-color: #032d00;
  }

  #app {
    .game-tiles {
      .search {
        display: flex;
        justify-content: center;
        padding: 24px 0;
        position: relative;
        width: 100%;

        input {
          font-size: 1.5rem;
          text-align: center;
          width: 90%;
        }

        .clear-search {
          align-items: center;
          display: flex;
          font-size: 1.3rem;
          height: 100%;
          position: absolute;
          right: 6%;
          top: 0;

          i {
            color: black;
            cursor: pointer;

            @media screen and (min-width: $md) {
              color: #b5b5b5;

              &:hover {
                color: black;
              }
            }
          }
        }
      }

      display: flex;
      flex-wrap: wrap;
    }
  }
</style>
