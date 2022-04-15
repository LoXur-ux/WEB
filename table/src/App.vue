<template>
  <MyTable />
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import MyTable from './components/MyTable.vue';

@Options({
  components: {
    MyTable,
  },
})
export default class App extends Vue {}
</script>
