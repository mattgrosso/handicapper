<template>
  <div class="game-tile" @click="$emit('selected')">
    <div class="image-wrapper">
      <img :src="game.thumbnail" :alt="game.title">
    </div>
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
    justify-content: space-between;
    margin: 12px;
    padding: 12px;
    width: calc(50% - 24px);

    &:hover {
      background: black;
      color: white;

      img {
        border: 1px solid #353535;
      }
    }

    @media screen and (min-width: $sm) {
      padding: 24px 12px;
      width: calc(33% - 24px);
    }

    @media screen and (min-width: $md) {
      width: calc(20% - 24px);
    }

    .image-wrapper {
      align-items: center;
      display: flex;
      flex: auto;

      img {
        border: 1px solid transparent;
        max-width: 100%;
      }
    }

    .title {
      font-family: 'Tinos', serif;
      font-size: 1.5rem;
      hyphens: auto;
      margin-top: 12px;
      text-align: center;

      @media screen and (min-width: $sm) {
        margin-top: 24px;
      }
    }
  }
</style>
