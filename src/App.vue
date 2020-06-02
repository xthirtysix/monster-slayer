<template>
  <div id="app" class="container">
    <h1 class="main-header">{{ name }}</h1>
    <Rivals/>
  </div>
</template>

<script>
import Rivals from './components/Rivals.vue';

export default {
  name: 'Monster Slayer',
  data() {
    return {
      name: 'Monster Slayer',
    };
  },
  components: {
    Rivals,
  },
};
</script>


<style lang="scss">
  @import '@/scss/_mixins.scss';

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
