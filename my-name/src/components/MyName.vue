<template>
  <div class="name">
    <h3>{{ lstName }} {{ firName }} {{ midName }}</h3>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default {
  name: "MyName",
  props: {
    firName: String,
    midName: String,
    lstName: String,
  },
};
</script>

<style scoped>
.name {
  text-align: center;
  color: blue;
}
</style>
