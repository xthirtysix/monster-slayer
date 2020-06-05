<template>
  <section class="log" v-if="isGameRunning && log.length">
    <h2 class="visually-hidden">Battle log</h2>
    <ul class="log__list">
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
@import '../scss/_base.scss';
@import '../scss/_variables.scss';
@import '../scss/_mixins.scss';

.log {
  @include border;
  max-width: 400px;
  margin: 0 auto;
}

.log__list {
  @include list-reset;
  max-height: 300px;
  padding: 0.5rem 1rem;
  overflow: auto;
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

</style>
