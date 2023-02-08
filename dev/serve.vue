<script>
import Vue from 'vue';
import ConicalGradientProgress from '@/conical-gradient-progress.vue';

export default Vue.extend({
  name: 'ServeDev',
  components: {
    ConicalGradientProgress
  }
});
</script>

<template>
  <div id="app">
    <conical-gradient-progress />
  </div>
</template>
