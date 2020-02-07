<template>
  <div class="player-row">
    <p class="name">{{player.name}}</p>
    <p class="number">{{player.number}}</p>
    <p class="wins">{{player.wins}}</p>
    <p class="average">{{player.average}}</p>
    <p class="handicap">{{handicap}}</p>
  </div>
</template>

<script>
  const component = {
    props: {
      player: Object,
      handicap: Number
    }
  };

  export default component;
</script>

<style lang="scss" scoped>
</style>
