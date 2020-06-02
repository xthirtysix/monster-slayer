<template>
  <div id="app" class="container">
    <h1 class="main-header">{{ name }}</h1>
    <Rivals
      :playerHP="playerHealth"
      :monsterHP="monsterHealth"/>
    <Actionbar
      @start-game="startGame"
      @attack="attack"
      :running="isRunning"/>
  </div>
</template>

<script>
import Rivals from './components/Rivals.vue';
import Actionbar from './components/Actionbar.vue';
import { MAX_HEALTH, MAX_PERCENT, DAMAGE } from './config/constants';

export default {
  data() {
    return {
      name: 'Monster Slayer',
      isRunning: false,
      playerHealth: MAX_HEALTH.PLAYER,
      monsterHealth: MAX_HEALTH.MONSTER,
    };
  },
  components: {
    Rivals,
    Actionbar,
  },
  methods: {
    startGame() {
      this.isRunning = true;
      this.playerHealth = (MAX_HEALTH.PLAYER * MAX_PERCENT) / MAX_HEALTH.PLAYER;
      this.monsterHealth = (MAX_HEALTH.MONSTER * MAX_PERCENT) / MAX_HEALTH.MONSTER;
    },
    calcDamage(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    attack() {
      // Calculate percent player health left
      this.playerHealth -= Math.floor((
        this.calcDamage(
          DAMAGE.MONSTER.MIN,
          DAMAGE.MONSTER.MAX,
        )
        * MAX_PERCENT)
        / MAX_HEALTH.PLAYER);

      // Calculate percent of monster health left
      this.monsterHealth -= Math.round((
        this.calcDamage(
          DAMAGE.PLAYER.MIN,
          DAMAGE.PLAYER.MAX,
        )
        * MAX_PERCENT)
        / MAX_HEALTH.MONSTER);

      if (this.playerHealth <= 0) {
        this.isRunning = false;
      }
    },
    specialAttack() {},
    heal() {},
    run() {},
  },
};
</script>


<style lang="scss">
  @import './scss/_mixins.scss';

  #app {
    @include border;
    margin-top: 60px;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
  }

  h1 {
    text-transform: uppercase;
  }
</style>
