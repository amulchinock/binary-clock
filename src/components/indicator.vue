<template>
  <div class="indicator" :class="{ on: state === true }"></div>
</template>

<script>
export default {
  props: {
    state: {
      type: Boolean,
      default: false,
      required: false
    }
  }
}
</script>

<style lang="scss">
.indicator {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  border: 1px solid black;
  align-self: center;
  margin: 10px;

  &.on {
    background: green;
  }
}
</style>