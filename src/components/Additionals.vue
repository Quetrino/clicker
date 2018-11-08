<template>
  <div class="additionals">
    <div class="menu">
      <div class="option"
           @click="$emit('adds-click')">Adds</div>
      <div class="option"
           @click="$emit('opts-click')">Options</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Additionals',
};
</script>

<style lang="scss">
  .menu {
    width: 100%;
    display: flex;
    border-bottom: 1px solid white;

    .option {
      width: 100%;
      background: #000;
      text-align: center;
      line-height: 10vh;
      border-left: 1px solid white;
      transform: rotate(360deg);
      &:hover, &:focus {
        color: #61a;
        cursor: pointer;
      }
    }

    .option:first-child {
      border: none;
    }
  }
  @media all and (min-width: 1200px) {
    .option {
      font-size: 5vh;
    }
  }
</style>
