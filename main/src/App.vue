<template>
  <div id="app">
    <MyName firName="Данил" midName="Александрович" lstName="Панишев" class="block" />
    <Vyatsu class="block" hidden/>
    <Login class="block" hidden/>
    <Table class="block"/>
    <Button class="blick" />
  </div>
</template>

<script lang="ts">
import MyName from '../../my-name/src/components/MyName.vue';
import Vyatsu from '../../vyatsu/src/components/Vyatsu.vue';
import Login from '../../login/src/components/Login.vue';
import Table from '../../table/src/components/Table.vue';
import Button from '../../button/src/components/Button.vue';

export default {
  name: 'App',
  components: {
    MyName,
    Vyatsu,
    Login,
    Table,
    Button,
  },
};

</script>

<style scoped>
.block {
  margin: 0 auto 16px auto;
}
</style>
