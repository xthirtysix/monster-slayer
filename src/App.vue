<template>
  <div id="app" class="container">
    <h1 class="main-header">{{ name }}</h1>
    <Rivals
      :playerHP="playerHealth"
      :monsterHP="monsterHealth"
      :status="status"/>
    <Actionbar
      @startGame="startGame"
      @attack="roundWithAttack"
      @specialAttack="roundWithSpecialAttack"
      @heal="roundWithHeal"
      @run="run"
      :isGameRunning="isGameRunning"/>
    <Battlelog
      :log="log"
      :isGameRunning="isGameRunning"/>
  </div>
</template>

<script>
import Rivals from './components/Rivals.vue';
import Actionbar from './components/Actionbar.vue';
import Battlelog from './components/Battlelog.vue';

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
      log: [],
      status: 'Game is not started',
    };
  },
  components: {
    Rivals,
    Actionbar,
    Battlelog,
  },
  methods: {
    startGame() {
      this.log = [];
      this.isGameRunning = true;
      this.playerHealth = (MAX_HEALTH.PLAYER * MAX_PERCENT) / MAX_HEALTH.PLAYER;
      this.monsterHealth = (MAX_HEALTH.MONSTER * MAX_PERCENT) / MAX_HEALTH.MONSTER;
      this.status = 'Fight!';
    },
    calcDamage(min, max) {
      return Math.max(Math.floor(Math.random() * max) + 1, min);
    },
    playerAttack() {
      const damageDealt = this.calcDamage(DAMAGE.PLAYER.MIN, DAMAGE.PLAYER.MAX);

      this.monsterHealth -= Math.round((
        damageDealt
        * MAX_PERCENT)
        / MAX_HEALTH.MONSTER);

      return damageDealt;
    },
    playerSpecialAttack() {
      const damageDealt = this.calcDamage(DAMAGE.PLAYER.SPECIAL.MIN, DAMAGE.PLAYER.SPECIAL.MAX);

      this.monsterHealth -= Math.round((
        damageDealt
        * MAX_PERCENT)
        / MAX_HEALTH.MONSTER);

      return damageDealt;
    },
    monsterAttack() {
      const damageDealt = this.calcDamage(DAMAGE.MONSTER.MIN, DAMAGE.MONSTER.MAX);
      this.playerHealth -= Math.round((
        damageDealt
        * MAX_PERCENT)
        / MAX_HEALTH.PLAYER);

      return damageDealt;
    },
    heal() {
      const healthPercent = Math.round((HEALTH_RESTORED * MAX_PERCENT) / MAX_HEALTH.PLAYER);

      if (this.playerHealth + healthPercent >= 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += healthPercent;
      }

      return HEALTH_RESTORED;
    },
    run() {
      this.isGameRunning = false;
      this.status = 'You flee the battle... how dare You?';
    },
    round(playerAttackType) {
      // Player turn
      const playerHit = playerAttackType();

      let attackType;

      switch (playerAttackType) {
        case this.playerSpecialAttack:
          attackType = 'attacks hard';
          break;
        case this.heal:
          attackType = 'heals';
          break;
        default:
          attackType = 'attacks';
      }

      this.log.unshift({
        subject: 'Player', subjectDoes: attackType, value: playerHit, key: `player${Date.now()}`,
      });

      if (this.monsterHealth <= 0) {
        this.monsterHealth = 0;
        this.status = 'You win!';
        this.isGameRunning = false;
        return;
      }

      // Monster turn
      const monsterHit = this.monsterAttack();

      this.log.unshift({
        subject: 'Monster', subjectDoes: 'attacks', value: monsterHit, key: `monster${Date.now()}`,
      });

      if (this.playerHealth <= 0) {
        this.playerHealth = 0;
        this.status = 'You loose!';
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
