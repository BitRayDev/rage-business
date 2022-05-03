<template>
  <div class="business-information-tab">
    <img class="business-information-tab__image" :src="business.image"/>
    <div class="business-information-tab__properties-container">
      <div class="business-information-tab__properties-row">
        <BriefProperty class="business-information-tab__property" title="Название" theme="blue-text"
                       :value="business.name"/>
        <BriefProperty class="business-information-tab__property" title="Тип" theme="blue-text" :value="business.type"/>
      </div>
      <div class="business-information-tab__properties-row">
        <BriefProperty class="business-information-tab__property" title="Гос. цена" theme="blue-text"
                       :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')"
                       :value="business.statePrice"/>
        <BriefProperty class="business-information-tab__property" title="Аренда" theme="blue-text"
                       :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')"
                       :value="business.rentalPrice"/>
        <BriefProperty class="business-information-tab__property" title="Налог" theme="blue-text"
                       :value="business.fees"/>
      </div>
    </div>
    <div class="business-information-tab__balance business-balance">
      <p class="business-balance__label">Баланс</p>
      <p class="business-balance__value">{{ business.balance }}</p>
    </div>
    <div class="business-information-tab__buttons">
      <div class="business-information-tab__buttons-section">
        <Button class="business-information-tab__button business-information-tab__button_type_withdraw" @click.native="onWithdrawButtonClick">
          Снять деньги
        </Button>
        <Button class="business-information-tab__button business-information-tab__button_type_top-up" @click.native="onTopUpButtonClick">
          Пополнить счет
        </Button>
      </div>
      <Button class="business-information-tab__button business-information-tab__button_type_sell" @click.native="onSellButtonClick">Продать бизнес
      </Button>
    </div>
    <SellBusinessModal :business="business" v-if="isSellModalActive" @cancel="isSellModalActive = false"/>
  </div>
</template>

<script>
import BriefProperty from "../../molecules/BriefProperty";
import Button from "../../atoms/Button";
import SellBusinessModal from "../../organisms/SellBusinessModal";

export default {
  name: "BusinessInformationTab",
  components: {SellBusinessModal, Button, BriefProperty},
  props: {
    business: Object
  },
  data: function () {
    return {
      isSellModalActive: false,
    }
  },
  methods: {
    onWithdrawButtonClick: function () {

    },
    onTopUpButtonClick: function () {

    },
    onSellButtonClick: function () {
      this.isSellModalActive = true;
    },
  }
}
</script>

<style lang="scss" scoped>
.business-balance {
  &__label {
    color: #D2D2D6;

    font-size: .7vw;
    font-weight: 500;
  }
  &__value {
    color: white;

    font-size: 1.75vw;
    font-weight: 700;
  }
}

.business-information-tab {
  display: flex;
  flex-direction: column;
  gap: .5vw;

  overflow: hidden;

  height: 100%;

  &__image {
    flex-grow: 1;

    display: block;

    min-height: 0;

    object-fit: cover;

    border-radius: 1vw;
  }

  &__properties-container {
    display: flex;
    flex-direction: column;
    gap: .75vw;

    margin-bottom: .5vw;
  }

  &__properties-row {
    display: flex;
    gap: .5vw;
  }

  &__property {
    flex-grow: 1;
  }

  &__buttons {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: .5vw;
  }
  &__buttons-section {
    display: flex;
    align-items: center;
    gap: .5vw;
  }
  &__button {
    height: 2.25vw;
    width: 10vw;

    box-sizing: border-box;
    padding: 0;

    &_type {
      &_withdraw {
        background-color: #02CE0A;
        border: none;
      }
      &_sell {
        background-color: #29202D;
        border: .1vw solid rgba(255, 255, 255, 0.1);
      }
    }
  }
}
</style>