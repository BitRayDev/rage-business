<template>
  <input class="checkbox" type="checkbox" />
</template>

<script>
export default {
  name: "Checkbox"
}
</script>

<style lang="scss" scoped>
.checkbox {
  -webkit-appearance: none;

  height: 1vw;
  width: 1vw;

  border: .1vw solid #37B0FF;
  filter: drop-shadow(0px 0px .5vw rgba(0, 164, 255, 0.4)) drop-shadow(0px 0px 1.5vw rgba(0, 164, 255, 0.2));
  border-radius: .2vw;

  &:checked {
    background-image: url("../../assets/business/img/icons/checkbox-marked.svg");
    background-size: 70%;
    background-position: 40% 40%;
    background-repeat: no-repeat;
  }
}
</style>