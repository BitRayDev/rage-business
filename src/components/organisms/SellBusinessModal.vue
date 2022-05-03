<template>
  <div class="sell-business-modal">
    <BusinessModal class="sell-business-modal__modal" @close="$emit('cancel')">
      <template #header>
        Продать бизнес
      </template>
      <p class="sell-business-modal__text">Вы точно хотите продать бизнес государству?</p>
      <BriefProperty class="business-information-tab__property" title="Гос. цена" theme="blue-text"
                     :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')"
                     :value="business.statePrice"/>
      <div class="sell-business-modal__buttons">
        <Button class="sell-business-modal__button sell-business-modal__button_submit" @click.native="$emit('submit')">Продать</Button>
        <Button class="sell-business-modal__button sell-business-modal__button_cancel" @click.native="$emit('cancel')">Отмена</Button>
      </div>
    </BusinessModal>
  </div>
</template>

<script>
import BriefProperty from "../molecules/BriefProperty";
import Button from "../atoms/Button";
import BusinessModal from "./BusinessModal";

export default {
  name: "SellBusinessModal",
  components: {BusinessModal, Button, BriefProperty},
  props: {
    business: Object,
  }
}
</script>

<style lang="scss" scoped>
.sell-business-modal {
  display: flex;
  justify-content: center;
  align-items: center;

  height: 100%;
  width: 100%;

  position: absolute;
  z-index: 10;
  left: 0;
  top: 0;

  background: linear-gradient(180deg, rgba(0, 0, 0, 0.5) 0%, rgba(0, 0, 0, 0) 43.4%), radial-gradient(50% 88.89% at 50% 50%, rgba(0, 0, 0, 0) 0%, rgba(0, 0, 0, 0.6) 100%);
  backdrop-filter: blur(.3vw);

  &__modal {
    display: flex;
    flex-direction: column;

    gap: .5vw;
  }

  &__text {
    color: #D2D2D6;

    font-size: .75vw;
    font-weight: 700;
  }

  &__buttons {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: .5vw;
  }
  &__button {
    padding: .5vw 1vw;
    &_submit {
      background: linear-gradient(180deg, #66030B 0%, #590008 100%);
      border: .1vw solid rgba(255, 255, 255, 0.2);
    }
    &_cancel {
      background: rgba(55, 176, 255, 0.1);
      border: .1vw solid rgba(255, 255, 255, 0.05);
    }
  }
}
</style>