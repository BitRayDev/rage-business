<template>
  <div class="business-brief">
    <BusinessModal class="business-brief__modal">
      <template #header>
        {{ business.name }}
      </template>

      <div class="business-brief__properties-container">
        <div class="business-brief__properties-row">
          <BriefProperty class="business-brief__property" title="Владелец" :theme="business.owner ? '' : 'green'" :icon="require('@/assets/business/img/icons/user.svg')" :value="business.owner || 'Государство'" />
          <BriefProperty class="business-brief__property" title="Тип магазина" theme="blue-text" :value="business.type" />
        </div>
        <div class="business-brief__properties-row">
          <BriefProperty class="business-brief__property" title="Гос. цена" theme="blue-text" :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')" :value="business.statePrice" />
          <BriefProperty class="business-brief__property" title="Аренда" theme="blue-text" :icon="require('@/assets/business/img/icons/dollar-sign_blue.svg')" :value="business.rentalPrice" />
          <BriefProperty class="business-brief__property" title="Налог" theme="blue-text" :value="business.fees" />
        </div>
        <div class="business-brief__properties-row">
          <BriefProperty class="business-brief__property" title="Крыша" theme="blue-text" :value="business.criminalRoof" />
        </div>
      </div>
      <div class="business-brief__buttons">
        <Button class="business-brief__button business-brief__button_type_buy" v-if="!business.owner">Купить</Button>
        <Button class="business-brief__button business-brief__button_type_cancel">Выйти</Button>
      </div>
    </BusinessModal>
  </div>
</template>

<script>
import BusinessModal from "../organisms/BusinessModal";
import BriefProperty from "../molecules/BriefProperty";
import Button from "../atoms/Button";
export default {
  name: "BusinessBrief",
  components: {Button, BriefProperty, BusinessModal},
  data: function () {
    return {
      business: {
        name: "Магазин 24/7 №10",
        owner: "",
        type: "Магазин 24/7",
        statePrice: "55 000 000$",
        rentalPrice: "5 000$",
        fees: "15%",
        criminalRoof: "Армянская мафия",
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.business-brief {
  display: flex;
  align-items: center;
  justify-content: center;

  height: 100vh;

  &__modal {
    width: 26vw;
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
    flex: 1 1 0px;
  }

  &__buttons {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: .25vw;
  }
  &__button {
    width: 6vw;
    height: 2vw;

    &_type {
      &_buy {
        background: #37B0FF;
        border: .1vw solid rgba(255, 255, 255, 0.2);
        box-shadow: 0px 0px 1.5vw rgba(0, 164, 255, 0.05);
      }
      &_cancel {
        background: rgba(55, 176, 255, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.05);
        box-shadow: 0px 0px 1.5vw rgba(0, 164, 255, 0.05);
      }
    }
  }
}
</style>