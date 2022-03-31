<template>
  <div id="app">
    <MyName firName="Данил" midName="Александрович" lstName="Панишев" class="block" />
  </div>
</template>

<script lang="ts">
import MyName from '../../my-name/src/components/MyName.vue';

export default {
  name: 'App',
  components: {
    MyName,
  },
};
</script>

<style scoped>
.block {
  margin: 0 auto 16px auto;
}
</style>
