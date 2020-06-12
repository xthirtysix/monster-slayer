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
export default {
  props: {
    playerHP: Number,
    monsterHP: Number,
    status: String,
  },
};
</script>

<style lang="scss" scoped>
  @import '../scss/_mixins.scss';
  @import '../scss/_base.scss';

  .rivals {
    @include border;
  }

  ::-moz-progress-bar {
    transition: padding-bottom 1s;
    padding-bottom: var(--value);
    transform-origin: 0 0;
    transform: translateX(-15px);
    padding-left: 15px;
    height: 0;
  }

  progress {
    overflow: hidden;
  }

  h3 {
    margin: 0;
  }

  ul {
    @include list-reset;
  }

  .rivals__list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-column-gap: 1rem;
  }
</style>
