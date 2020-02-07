<template>
  <div class="game-tile" @click="$emit('selected')">
    <img :src="game.thumbnail" :alt="game.title">
    <div class="title">{{game.title}}</div>
  </div>
</template>

<script>
  const component = {
    props: {
      game: Object
    }
  };

  export default component;
</script>

<style lang="scss" scoped>
  @import "../assets/styles/variables.scss";

  .game-tile {
    align-items: center;
    background-color: #2b2b2b;
    border-radius: 5px;
    border: 1px solid black;
    box-sizing: border-box;
    color: white;
    cursor: pointer;
    display: flex;
    flex-direction: column;
    margin: 12px;
    padding: 24px 12px;
    width: calc(100% - 24px);

    &:hover {
      background: black;
      color: white;

      img {
        border: 1px solid #353535;
      }
    }

    @media screen and (min-width: $sm) {
      width: calc(50% - 24px);
    }

    @media screen and (min-width: $md) {
      width: calc(33% - 24px);
    }

    @media screen and (min-width: $lg) {
      width: calc(20% - 24px);
    }

    img {
      border: 1px solid transparent;
      margin-bottom: 24px;
      max-width: 100%;
    }

    .title {
      font-family: 'Tinos', serif;
      font-size: 1.5rem;
      hyphens: auto;
      text-align: center;
    }
  }
</style>
