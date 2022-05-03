<template>
  <div class="radio-button" @click="onClick">
    <div class="radio-button__inner" v-if="checked"/>
  </div>
</template>

<script>
export default {
  name: "RadioButton",
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  methods: {
    onClick: function () {
      this.$emit('change', !this.checked)
    }
  }
}
</script>

<style lang="scss" scoped>
.radio-button {
  display: flex;
  justify-content: center;
  align-items: center;

  height: .65vw;
  width: .65vw;

  border: .15vw solid #0A9CFB;
  border-radius: 100vw;

  &__inner {
    height: 75%;
    width: 75%;

    background-color: #0A9CFB;
    border-radius: 100vw;
  }
}
</style>