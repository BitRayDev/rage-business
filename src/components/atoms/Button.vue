<template>
  <button class="button">
    <slot/>
  </button>
</template>

<script>
export default {
  name: "Button"
}
</script>

<style lang="scss" scoped>
.button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: .25vw;

  padding: .35vw .75vw;

  color: white;

  font-size: .8vw;
  font-weight: 600;

  background: none;

  border: .1vw solid white;
  border-radius: .6vw;

  cursor: pointer;
}
</style>