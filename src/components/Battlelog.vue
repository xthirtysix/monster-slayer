<template>
  <section class="log" v-if="isGameRunning && log.length">
    <h2 class="visually-hidden">Battle log</h2>
    <ul class="log__list">
      <transition-group name="slide" type="animation">
        <li
          class="action"
          :class="{'action--monster': action.subject === 'Monster'}"
          :key="action.key"
          v-for="action in log">
          <p>
            <span class="action__subject">
              {{ action.subject }}
            </span>
            {{ action.subjectDoes }} for
            <span
              class="action__value"
              :class="{'action__value--heal': action.subjectDoes === 'heals'}">
              {{ action.value }}
            </span>
          </p>
        </li>
      </transition-group>
    </ul>
  </section>
</template>

<script>
export default {
  props: {
    isGameRunning: Boolean,
    log: Array,
  },
};
</script>

<style lang="scss">
@import '@/scss/_base.scss';
@import '@/scss/_variables.scss';
@import '@/scss/_mixins.scss';

.log {
  @include border;
  max-width: 400px;
  margin: 0 auto;
}

.log__list {
  @include list-reset;
  max-height: 300px;
  padding: 0.5rem 1rem;
  overflow-x: hidden;
  overflow-y: auto;
}

p {
  .action & {
    margin: 0;
    padding: 0.3rem;
    text-align: left;
    border-radius: 5px;
  }

  .action--monster & {
    text-align: right;
    background-color: var(--gray);
  }
}

.action__subject {
  font-weight: 700;
}

.action__value {
  color: var(--red);

  &--heal {
    color: var(--green);
  }
}

.slide {
  &-enter {
    opacity: 0.3;
  }

  &-enter-active {
    animation: slide-in 1s ease-out forwards;
    transition: opacity 1s;
  }

  &-move {
    transition: transform .5s;
  }
}

@keyframes slide-in {
  from {
    transform: translateY(-20px);
  }
  to {
    transform: translateY(0);
  }
}

</style>
