<template>
  <header>
    <h1>TodoList !</h1>
    <h2>(author: ChoiSuhyeon)</h2>
  </header>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "nuxt-property-decorator";
@Component
export default class TodoHeader extends Vue {}
</script>

<style scoped>
h1 {
  color: #f2f0f5;
  font-weight: 1900;
  margin: 2.5em 0 rem;
}
h2 {
  color: rgb(203, 255, 250);
}
</style>
