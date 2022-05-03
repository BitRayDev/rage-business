<template>
  <div class="drop-down-list">
    <div class="drop-down-list__header" @click="toggleExpand">
      <p class="drop-down-list__selected-option">{{ selectedOption }}</p>
      <img class="drop-down-list__expand-arrow" src="@/assets/business/img/icons/arrow.svg"/>
    </div>
    <div class="drop-down-list__options-list" v-if="isExpanded">
      <div class="drop-down-list__option" v-for="option in options" @click="selectOption(option)">
        <RadioButton :checked="option === selectedOption"/>
        <p class="drop-down-list__option-label">
          {{ option }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import RadioButton from "../atoms/RadioButton";

export default {
  name: "DropDownList",
  components: {RadioButton},
  model: {
    prop: "selectedOption",
    event: "select"
  },
  props: {
    selectedOption: String,
    options: Array,
  },
  data: function () {
    return {
      isExpanded: false,
    }
  },
  methods: {
    toggleExpand: function () {
      this.isExpanded = !this.isExpanded;
    },
    selectOption: function (option) {
      this.$emit('select', option);
      this.toggleExpand();
    }
  },
}
</script>

<style lang="scss" scoped>
.drop-down-list {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: .25vw;

  position: relative;
  z-index: 1;

  user-select: none;

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: .5vw;

    width: 6.75vw;
    padding: .35vw .4vw;

    background: rgba(55, 176, 255, 0.1);
    border: .1vw solid rgba(255, 255, 255, 0.05);
    border-radius: .3vw;
  }

  &__selected-option {
    color: white;

    font-size: .6vw;
    font-weight: 600;
  }

  &__expand-arrow {
    height: .35vw;
  }

  &__options-list {
    display: flex;
    flex-direction: column;
    gap: .25vw;

    position: absolute;
    top: 2vw;

    width: 100%;

    padding: .5vw;

    background: #141B23;
    border: .1vw solid rgba(255, 255, 255, 0.1);
    border-radius: .4vw;
  }

  &__option {
    display: flex;
    align-items: center;
    gap: .5vw;

    padding: .25vw .5vw;

    background: rgba(55, 176, 255, 0.02);
    border-radius: .3vw;
  }
  &__option-label {
    color: white;

    font-size: .6vw;
  }
}
</style>