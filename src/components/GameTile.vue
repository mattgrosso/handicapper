<template>
  <div class="game-tile" @click="$emit('selected')">
    <h2>{{game.title}}</h2>
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
  .game-tile {
    border: 1px solid grey;
    cursor: pointer;
    margin: 12px;
    padding: 12px;

    &:hover {
      background: black;
      color: white;
    }
  }
</style>
