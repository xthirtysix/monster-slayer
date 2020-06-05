<template>
  <span class="game-status">
    {{ status }}
  </span>
</template>

<script>
export default {
  props: {
    status: String,
  },
};
</script>

<style>
.game-status {
  text-transform: uppercase;
}
</style>
