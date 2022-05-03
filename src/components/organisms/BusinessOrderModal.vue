<template>
  <div class="business-order-modal">
    <BusinessModal class="business-order-modal__modal" @close="$emit('cancel')">
      <template #header>
        Оформить заказ
      </template>
      <p class="business-order-modal__text">Вы точно хотите оформить заказ на доставку товаров?</p>
      <div class="business-order-modal__details">
        <div class="business-order-modal__detail">
          <img class="business-order-modal__detail-icon" src="@/assets/business/img/icons/cart.svg">
          Стоимость продуктов: {{ itemsPrice }}$
        </div>
        <div class="business-order-modal__detail">
          <img class="business-order-modal__detail-icon"
               src="@/assets/business/img/icons/truck.svg">
          Стоимость доставки: {{ deliveryPrice }}$
        </div>
      </div>

      <BriefProperty class="business-information-tab__property" title="Итого к оплате" theme="blue-text"
                     :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')"
                     :value="itemsPrice"/>
      <div class="business-order-modal__buttons">
        <Button class="business-order-modal__button business-order-modal__button_submit" @click.native="$emit('submit')">Оформить</Button>
        <Button class="business-order-modal__button business-order-modal__button_cancel" @click.native="$emit('cancel')">Отмена</Button>
      </div>
    </BusinessModal>
  </div>
</template>

<script>
import BriefProperty from "../molecules/BriefProperty";
import Button from "../atoms/Button";
import BusinessModal from "./BusinessModal";

export default {
  name: "BusinessOrderModal",
  components: {BusinessModal, Button, BriefProperty},
  props: {
    itemsPrice: Number,
    deliveryPrice: Number,
  }
}
</script>

<style lang="scss" scoped>
.business-order-modal {
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
  }

  &__text {
    margin-bottom: .75vw;

    color: #D2D2D6;

    font-size: .75vw;
    font-weight: 700;
  }

  &__details {
    display: flex;
    flex-direction: column;
    gap: .75vw;

    margin-bottom: .75vw;
  }
  &__detail {
    display: flex;
    gap: .4vw;

    color: white;

    font-size: .75vw;
    font-weight: 700;
  }
  &__detail-icon {
    height: .9vw;
  }

  &__buttons {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    gap: .5vw;

    margin-top: .75vw;
  }
  &__button {
    padding: .5vw 1vw;

    &_submit {
      background: radial-gradient(50% 50% at 50% 50%, #37B0FF 0%, #0A9CFB 100%);
      border: .1vw solid rgba(255, 255, 255, 0.2);
    }
    &_cancel {
      background: rgba(55, 176, 255, 0.1);
      border: .1vw solid rgba(255, 255, 255, 0.05);
    }
  }
}
</style>