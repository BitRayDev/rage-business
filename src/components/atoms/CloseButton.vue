<template>
  <div class="close-button">
    <img class="close-button__icon" src="@/assets/business/img/icons/close.svg"/>
  </div>
</template>

<script>
export default {
  name: "CloseButton"
}
</script>

<style lang="scss" scoped>
.close-button {
  display: flex;
  align-items: center;
  justify-content: center;

  width: fit-content;
  height: fit-content;

  padding: .2vw;

  background: rgba(55, 176, 255, 0.1);

  border: .1vw solid rgba(255, 255, 255, 0.05);
  border-radius: 6px;

  transition: box-shadow 100ms ease-out;

  &:hover {
    box-shadow: 0 0 .5vw rgba(55, 176, 255, 0.1);
  }

  &__icon {
    height: 75%;
  }
}
</style>