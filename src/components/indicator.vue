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

<style lang="scss" scoped>
.indicator {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  border: 1px solid black;
  align-self: center;
  margin: 10px;
  background-color: transparent;
  transition: background-color 0.2s ease;
  position: relative;

  &:before {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    z-index: -1;
    border-radius: 50%;
    filter: blur(5px);
    background-color: transparent;

  }

  &.on {
    background-color: #33658A;

    &:before {
      content: '';
      background-color: #86BBD8;
      transition: 0.2s ease;
    }

  }
}
</style>