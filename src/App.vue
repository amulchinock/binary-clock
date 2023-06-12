<script setup>
import dotMatrix from './clocks/dot-matrix.vue';
</script>

<template>
  <section>
    <h1>Binary Clock</h1>
    <p>
      A very nerdy way of representing time! 🤓
    </p>
  </section>

  <section>
    <dot-matrix/>
  </section>
</template>

<style lang="scss">
body {
  background: #A22522;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;

  h1, p {
    margin: 0;
  }

  section {
    margin-bottom: 1.5rem;

    &:last-of-type {
      margin-bottom: 0;
    }
  }
}

#app {
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
