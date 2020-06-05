<template>
  <div id="app" class="container">
    <h1 class="main-header">{{ name }}</h1>
    <Gamestatus :status="status"/>
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
import Gamestatus from './components/Gamestatus.vue';
import Rivals from './components/Rivals.vue';
import Actionbar from './components/Actionbar.vue';
import Battlelog from './components/Battlelog.vue';

import {
  MAX_HEALTH,
  HEALTH_RESTORED,
  MAX_PERCENT,
  DAMAGE,
} from './config/constants';

import randomInRange from './utils/randomInRange';
import calcPercent from './utils/calcPercent';

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
    Gamestatus,
    Rivals,
    Actionbar,
    Battlelog,
  },
  methods: {
    startGame() {
      this.log = [];
      this.isGameRunning = true;
      this.playerHealth = MAX_PERCENT;
      this.monsterHealth = MAX_PERCENT;
      this.status = 'Fight!';
    },
    playerAttack() {
      const damageDealt = randomInRange(DAMAGE.PLAYER.MIN, DAMAGE.PLAYER.MAX);

      this.monsterHealth -= calcPercent(damageDealt, MAX_HEALTH.MONSTER);

      return damageDealt;
    },
    playerSpecialAttack() {
      const damageDealt = randomInRange(DAMAGE.PLAYER.SPECIAL.MIN, DAMAGE.PLAYER.SPECIAL.MAX);

      this.monsterHealth -= calcPercent(damageDealt, MAX_HEALTH.MONSTER);

      return damageDealt;
    },
    monsterAttack() {
      const damageDealt = randomInRange(DAMAGE.MONSTER.MIN, DAMAGE.MONSTER.MAX);

      this.playerHealth -= calcPercent(damageDealt, MAX_HEALTH.PLAYER);

      return damageDealt;
    },
    heal() {
      const healthRestored = randomInRange(HEALTH_RESTORED.MIN, HEALTH_RESTORED.MAX);

      const restoredInPercent = calcPercent(healthRestored, MAX_HEALTH.PLAYER);

      if (this.playerHealth + restoredInPercent >= 100) {
        this.playerHealth = 100;
      } else {
        this.playerHealth += restoredInPercent;
      }

      return healthRestored;
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
