<template>
  <div id="app">
    <MyName firName="Данил" midName="Александрович" lstName="Панишев" class="block" />
    <Vyatsu class="block"/>
  </div>
</template>

<script lang="ts">
import MyName from '../../my-name/src/components/MyName.vue';
import Vyatsu from '../../vyatsu/src/components/Vyatsu.vue';

export default {
  name: 'App',
  components: {
    MyName,
    Vyatsu,
  },
};
</script>

<style scoped>
.block {
  margin: 0 auto 16px auto;
}
</style>
