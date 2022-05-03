<template>
  <div class="tab-switch">
    <p
        :class="`tab-switch__option tab-switch-option ${option === selectedOption ? 'tab-switch-option_selected' : ''}`"
        v-for="option in options"
        @click="$emit('select', option)"
    >
      <img class="tab-switch-option__icon" :src="option.icon"/>
      {{option.title}}
    </p>
  </div>
</template>

<script>
export default {
  name: "TabSwitch",
  model: {
    prop: 'selectedOption',
    event: 'select'
  },
  props: {
    selectedOption: Object,
    options: Array
  }
}
</script>

<style lang="scss" scoped>
.tab-switch-option {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: .25vw;

  padding: .4vw .7vw;

  color: white;

  font-size: .9vw;
  font-weight: 600;

  background: rgba(55, 176, 255, 0.1);
  border: .1vw solid rgba(255, 255, 255, 0.1);
  border-radius: .5vw;

  cursor: pointer;

  transition: background-color 100ms ease-out;
  &_selected {
    background: #37B0FF;
    border-color: rgba(255, 255, 255, 0.2);
    box-shadow: 0px 0px 1.5vw rgba(0, 164, 255, 0.05);
  }

  &__icon {
    height: 1vw;
  }
}

.tab-switch {
  display: flex;
  gap: .5vw;

  &__option {
    flex: 1 1 0px;
  }
}
</style>