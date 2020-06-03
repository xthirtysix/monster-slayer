<template>
  <div id="app" class="container">
    <h1 class="main-header">{{ name }}</h1>
    <Rivals
      :playerHP="playerHealth"
      :monsterHP="monsterHealth"/>
    <Actionbar
      @startGame="startGame"
      @attack="roundWithAttack"
      @specialAttack="roundWithSpecialAttack"
      @heal="roundWithHeal"
      @run="run"
      :isGameRunning="isGameRunning"/>
  </div>
</template>

<script>
import Rivals from './components/Rivals.vue';
import Actionbar from './components/Actionbar.vue';
import {
  MAX_HEALTH,
  HEALTH_RESTORED,
  MAX_PERCENT,
  DAMAGE,
} from './config/constants';

export default {
  data() {
    return {
      name: 'Monster Slayer',
      isGameRunning: false,
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
      this.isGameRunning = true;
      this.playerHealth = (MAX_HEALTH.PLAYER * MAX_PERCENT) / MAX_HEALTH.PLAYER;
      this.monsterHealth = (MAX_HEALTH.MONSTER * MAX_PERCENT) / MAX_HEALTH.MONSTER;
    },
    calcDamage(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    playerAttack() {
      this.monsterHealth -= Math.round((
        this.calcDamage(
          DAMAGE.PLAYER.MIN,
          DAMAGE.PLAYER.MAX,
        )
        * MAX_PERCENT)
        / MAX_HEALTH.MONSTER);
    },
    playerSpecialAttack() {
      this.monsterHealth -= Math.round((
        this.calcDamage(
          DAMAGE.PLAYER.SPECIAL.MIN,
          DAMAGE.PLAYER.SPECIAL.MAX,
        )
        * MAX_PERCENT)
        / MAX_HEALTH.MONSTER);
    },
    monsterAttack() {
      this.playerHealth -= Math.round((
        this.calcDamage(
          DAMAGE.MONSTER.MIN,
          DAMAGE.MONSTER.MAX,
        )
        * MAX_PERCENT)
        / MAX_HEALTH.PLAYER);
    },
    heal() {
      const healthPercent = Math.round((HEALTH_RESTORED * MAX_PERCENT) / MAX_HEALTH.PLAYER);

      if (this.playerHealth + healthPercent >= 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healthPercent;
      }
    },
    run() {
      this.isGameRunning = false;
    },
    round(playerAttackType) {
      playerAttackType();
      this.monsterAttack();

      if (this.playerHealth <= 0 || this.monsterHealth <= 0) {
        this.isGameRunning = false;
      }
    },
    roundWithAttack() {
      this.round(this.playerAttack);
    },
    roundWithSpecialAttack() {
      this.round(this.playerSpecialAttack);
    },
    roundWithHeal() {
      this.round(this.heal);
    },
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
