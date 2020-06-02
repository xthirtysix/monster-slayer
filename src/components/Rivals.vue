<template>
  <section class="rivals">
    <h2 class="visually-hidden">Rivals</h2>
    <ul class="rivals__list">
      <li>
        <h3>You</h3>
        <progress
          id="player"
          max="100"
          :value="playerHealth.current">
          {{ playerHealth }}
        </progress>
      </li>
      <li>
        <h3>Monster</h3>
        <progress
          id="monster"
          max="100"
          :value="monsterHealth.current">
          {{ monsterHealth }}
        </progress>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  data() {
    const MAX_HEALTH = {
      PLAYER: 100,
      MONSTER: 150,
    };

    return {
      playerHealth: {
        max: MAX_HEALTH.PLAYER,
        current: this.calcHealthPercent(MAX_HEALTH.PLAYER, MAX_HEALTH.PLAYER),
      },
      monsterHealth: {
        max: MAX_HEALTH.MONSTER,
        current: this.calcHealthPercent(MAX_HEALTH.MONSTER, MAX_HEALTH.MONSTER),
      },
    };
  },
  methods: {
    calcHealthPercent(total, current) {
      return (current * 100) / total;
    },
  },
};
</script>

<style scoped lang="scss">
  @import '@/scss/_mixins.scss';
  @import '@/scss/_base.scss';

  .rivals {
    @include border;
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
