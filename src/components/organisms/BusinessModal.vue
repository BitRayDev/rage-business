<template>
  <div class="business-modal">
    <div class="business-modal__header business-modal-header">
      <div class="business-modal-header__content">
        <slot name="header"/>
      </div>
      <CloseButton :class="`business-modal__close-button ${theme ? `business-modal__close-button_theme_${theme}` : ''}`" @click.native="$emit('close')"/>
    </div>
    <slot/>
  </div>
</template>

<script>
import CloseButton from "../atoms/CloseButton";

export default {
  name: "BusinessModal",
  components: {CloseButton},
  props: {
    theme: String
  }
}
</script>

<style lang="scss" scoped>
.business-modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: .75vw;

  &__content {
    flex-grow: 1;

    color: white;

    font-weight: 600;
    font-size: .8vw;
  }
}

.business-modal {
  padding: .75vw;

  background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);

  border: .1vw solid rgba(255, 255, 255, 0.1);
  border-radius: 1vw;

  box-shadow: 0px 0px 2.5vw rgba(0, 164, 255, 0.05);

  &__header {
    margin-bottom: .5vw;
  }

  &__close-button {
    &_theme {
      &_menu {
        height: 1.5vw;
        width: 1.5vw;
      }
    }
  }
}

</style>