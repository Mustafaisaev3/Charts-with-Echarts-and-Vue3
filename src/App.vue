<template>
  <div>
    <Charts />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Charts from './components/Charts.vue';

export default defineComponent({
  name: 'App',
  components: {
    Charts
  }
});
</script>

<style>

</style>
