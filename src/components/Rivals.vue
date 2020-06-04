<template>
  <section class="rivals">
    <h2 class="visually-hidden">Rivals</h2>
    <ul class="rivals__list">
      <li>
        <h3>You</h3>
        <progress
          id="player"
          max="100"
          :value="playerHP">
          </progress>
      </li>
      <li class="game-status">
        <Gamestatus :status="status"/>
      </li>
      <li>
        <h3>Monster</h3>
        <progress
          id="monster"
          max="100"
          :value="monsterHP">
        </progress>
      </li>
    </ul>
  </section>
</template>

<script>
import Gamestatus from './Gamestatus.vue';

export default {
  props: {
    playerHP: Number,
    monsterHP: Number,
    status: String,
  },
  components: {
    Gamestatus,
  },
};
</script>

<style scoped lang="scss">
  @import '../scss/_mixins.scss';
  @import '../scss/_base.scss';

  .rivals {
    @include border;
  }

  h3 {
    margin: 0;
  }

  ul {
    @include list-reset;
  }

  .game-status {
    font-size: 0.8rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .rivals__list {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-column-gap: 1rem;
  }
</style>
