<template>
  <section class="action-bar" v-if="!running">
    <button
      class="action-bar__btn action-bar__btn--start"
      type="button"
      @click="$emit('start-end-game')">
      Start
    </button>
  </section>
  <section class="action-bar" v-else>
    <button
      class="action-bar__btn action-bar__btn--attack"
      type="button">
      Attack
    </button>
    <button
      class="action-bar__btn action-bar__btn--special"
      type="button">
      Special attack
    </button>
    <button
      class="action-bar__btn action-bar__btn--heal"
      type="button">
      Heal
    </button>
    <button
      class="action-bar__btn action-bar__btn--run"
      type="button"
      @click="$emit('start-end-game')">
      Run!
    </button>
  </section>
</template>

<script>
export default {
  props: {
    running: Boolean,
  },
  data() {
    return {
    };
  },
};
</script>

<style type="scss">
@import '../scss/_variables.scss';
.action-bar {
  display: grid;
  grid-template-columns: max-content max-content max-content max-content;
  grid-column-gap: 1rem;
  justify-content: center;
  padding: 2rem 0 1rem;
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
}

.action-bar__btn:hover {
  opacity: 0.7;
}

.action-bar__btn--start {
  background-color: var(--blue);
}

.action-bar__btn--attack {
    background-color: var(--red);
  }

.action-bar__btn--special {
    background-color: var(--red);
  }

.action-bar__btn--heal {
  background-color: var(--green);
}

.action-bar__btn--run {
  color: var(--black);
  background-color: var(--whie);
  border: 1px solid var(--black);
}
</style>
