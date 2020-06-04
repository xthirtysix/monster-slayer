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

<style type='scss'>
.game-status {
  text-transform: uppercase;
}
</style>
