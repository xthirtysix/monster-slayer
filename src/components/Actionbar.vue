<template>
  <section class="action-bar" v-if="!isGameRunning" :class="{'action-bar--start': !isGameRunning}">
    <button
      class="action-bar__btn action-bar__btn--start"
      type="button"
      @click="$emit('startGame')">
      New game
    </button>
  </section>
  <section class="action-bar" v-else>
    <button
      class="action-bar__btn action-bar__btn--attack"
      type="button"
      @click="$emit('attack')">
      Attack
    </button>
    <button
      class="action-bar__btn action-bar__btn--special"
      type="button"
      @click="$emit('specialAttack')">
      Special attack
    </button>
    <button
      class="action-bar__btn action-bar__btn--heal"
      type="button"
      @click="$emit('heal')">
      Heal
    </button>
    <button
      class="action-bar__btn action-bar__btn--run"
      type="button"
      @click="$emit('run')">
      Run!
    </button>
  </section>
</template>

<script>
export default {
  props: {
    isGameRunning: Boolean,
  },
};
</script>

<style lang="scss">
@import '../scss/_variables.scss';

.action-bar {
  display: grid;
  grid-template-columns: max-content max-content max-content max-content;
  grid-column-gap: 1rem;
  justify-content: center;
  padding: 2rem 0 1rem;
}

.action-bar--start {
  grid-template-columns: none;
}

.action-bar__btn {
  padding: 1rem 2rem;
  text-transform: uppercase;
  color: var(--white);
  background-color: transparent;
  border: none;
  border-radius: 5px;
  outline: none;
  transition: 0.3s ease-in-out;

  &:hover {
    opacity: 0.7;
  }

  &:active {
    opacity: 0.3;
  }

  &--start {
    background-color: var(--blue);
  }

  &--attack {
    background-color: var(--red);
  }

  &--special {
    background-color: var(--red);
  }

  &--heal {
    background-color: var(--green);
  }

  &--run {
    color: var(--black);
    background-color: var(--whie);
    border: 1px solid var(--black);
  }
}
</style>
