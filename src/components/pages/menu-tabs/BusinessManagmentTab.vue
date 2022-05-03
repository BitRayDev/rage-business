<template>
  <div class="business-management-tab">
    <div class="business-management-tab__extra-charge-edit business-extra-charge-edit">
      <div class="business-extra-charge-edit__brief-container">
        <img class="business-extra-charge-edit__brief-icon" src="@/assets/business/img/icons/percent.svg">
        <div class="business-extra-charge-edit__brief-text-container">
          <p class="business-extra-charge-edit__brief-title">
            Текущая наценка - {{ extraCharge.value }}%
          </p>
          <p class="business-extra-charge-edit__brief-subtitle">
            Минимальная наценка - {{ extraCharge.min }}%, Максимальная - {{ extraCharge.max }}%
          </p>
        </div>
      </div>
      <div class="extra-charge-input">
        <input class="extra-charge-input__input" type="number" placeholder="Введите процент наценки"
               v-model="extraCharge.inputValue"/>
        <div class="extra-charge-input__buttons">
          <img class="extra-charge-input__range-button" src="@/assets/business/img/icons/minimum.svg"
               @click="setExtraChargeInputToMin"/>
          <img class="extra-charge-input__range-button" src="@/assets/business/img/icons/maximum.svg"
               @click="setExtraChargeInputToMax"/>
          <Button class="extra-charge-input__save-button" @click.native="applyExtraCharge">Сохранить</Button>
        </div>
      </div>
    </div>
    <div class="business-management-tab__warehouse-management business-warehouse-management">
      <p class="business-warehouse-management__label business-warehouse-label">
        <img class="business-warehouse-label__icon" src="@/assets/business/img/icons/file.svg"/>
        Заявка на доставку
      </p>
      <div class="business-warehouse-management__top">
        <div class="business-search-input">
          <img class="business-search-input__icon" src="@/assets/business/img/icons/search.svg"/>
          <input class="business-search-input__input" type="text" placeholder="Поиск" v-model="searchValue"/>
        </div>
        <DropDownList :options="typeFilterOptions" v-model="typeFilterSelectedOption"/>
      </div>
      <div class="business-warehouse-management__table business-warehouse-table">
        <div class="business-warehouse-table__row business-warehouse-table__row_type_header">
          <div
              class="business-warehouse-table__cell business-warehouse-table__cell_type_header business-warehouse-table__cell_column_name">
            Наименование
          </div>
          <div
              class="business-warehouse-table__cell business-warehouse-table__cell_type_header business-warehouse-table__cell_column_remaining">
            Остаток, шт
          </div>
          <div
              class="business-warehouse-table__cell business-warehouse-table__cell_type_header business-warehouse-table__cell_column_price">
            Цена на продажу
          </div>
          <div
              class="business-warehouse-table__cell business-warehouse-table__cell_type_header business-warehouse-table__cell_column_original-price">
            Закупочная цена
          </div>
          <div
              class="business-warehouse-table__cell business-warehouse-table__cell_type_header business-warehouse-table__cell_column_buttons"></div>
        </div>

        <div class="business-warehouse-table__items">
          <div class="business-warehouse-table__row business-warehouse-table__row_type_item"
               :class="{'business-warehouse-table__row_active': item.orderPieces > 0}" v-for="item in filteredItems">
            <div
                class="business-warehouse-table__cell business-warehouse-table__cell_type_item business-warehouse-table__cell_column_name">
              {{ item.name }}
            </div>
            <div
                class="business-warehouse-table__cell business-warehouse-table__cell_type_item business-warehouse-table__cell_column_remaining">
              <p class="business-warehouse-table__value"
                 :class="{
                 'business-warehouse-table__value_theme_red': item.remainingPieces < item.remainingLimit * lowAmountThreshold,
                 'business-warehouse-table__value_theme_dark-green': item.remainingPieces > item.remainingLimit * highAmountThreshold
               }">
                {{ item.remainingPieces }}/{{ item.remainingLimit }}
              </p>
            </div>
            <div
                class="business-warehouse-table__cell business-warehouse-table__cell_type_item business-warehouse-table__cell_column_price">
              <p class="business-warehouse-table__value">
                {{ item.price }}$/шт
              </p>
            </div>
            <div
                class="business-warehouse-table__cell business-warehouse-table__cell_type_item business-warehouse-table__cell_column_original-price">
              <p class="business-warehouse-table__value" :class="{'business-warehouse-table__value_theme_dark-green': item.orderPieces > 0}">
                {{ item.orderPieces ? item.originalPrice * item.orderPieces : item.originalPrice }}$
              </p>
            </div>
            <div
                class="business-warehouse-table__cell business-warehouse-table__cell_type_item business-warehouse-table__cell_column_buttons">
              <div class="business-warehouse-table__buttons">
                <p class="business-warehouse-table__amount-button" @click="decrementItemOrderPieces(item)">-</p>
                <input class="business-warehouse-table__amount-input" type="number" v-model="item.orderPieces"/>
                <p class="business-warehouse-table__amount-button" @click="incrementItemOrderPieces(item)">+</p>
                <img class="business-warehouse-table__amount-max-button" src="@/assets/business/img/icons/maximum.svg"
                     @click="fillItemOrderPieces(item)"/>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="business-warehouse-management__bottom">
        <div class="business-warehouse-management__additional-buttons">
          <div class="business-warehouse-management__additional-button-container">
            <p class="business-warehouse-management__additional-button-label">Добавить в заявку все недостающие
              позиции</p>
            <Button class="business-warehouse-management__additional-button" @click.native="fillWarehouse">
              <img class="business-warehouse-management__additional-button-icon"
                   src="@/assets/business/img/icons/maximum.svg"/>
              Заполнить склад
            </Button>
          </div>
          <div class="business-warehouse-management__addition-button-container">
            <p class="business-warehouse-management__additional-button-label">Удалить все позиции из заявки</p>
            <Button class="business-warehouse-management__additional-button" @click.native="clearOrder">
              <img class="business-warehouse-management__additional-button-icon"
                   src="@/assets/business/img/icons/refresh.svg"/>
              Обнулить заказ
            </Button>
          </div>
        </div>
        <div class="business-warehouse-management__calculations-container">
          <div class="business-warehouse-management__calculations-section">
            <div class="business-warehouse-management__calculation">
              <img class="business-warehouse-management__calculation-icon" src="@/assets/business/img/icons/cart.svg">
              Стоимость продуктов: {{ orderedItemsPrice }}$
            </div>
            <div class="business-warehouse-management__calculation">
              <img class="business-warehouse-management__calculation-icon"
                   src="@/assets/business/img/icons/truck.svg">
              Стоимость доставки: {{ deliveryPrice }}$
            </div>
          </div>
          <div class="business-warehouse-management__total-container">
            <p class="business-warehouse-management__total-text">Итого к оплате:
              {{ orderedItemsPrice > 0 ? orderedItemsPrice + deliveryPrice : 0 }}$</p>
            <Button class="business-warehouse-management__order-button"
                    :class="{'business-warehouse-management__order-button_active': orderedItemsPrice > 0}"
                    @click.native="onOrderButtonClick">Оформить заказ
            </Button>
          </div>
        </div>
      </div>
    </div>
    <BusinessOrderModal
        :delivery-price="deliveryPrice"
        :items-price="orderedItemsPrice"
        v-if="isOrderModalActive"
        @cancel="isOrderModalActive = false"
    />
  </div>
</template>

<script>
import Button from "../../atoms/Button";
import DropDownList from "../../organisms/DropDownList";
import BusinessOrderModal from "../../organisms/BusinessOrderModal";

const testItem1 = {
  name: "Монтировка",
  originalPrice: 4500,
  remainingPieces: 5,
  remainingLimit: 50,
  price: 150,
  orderPieces: 0,
}
const testItem2 = {
  name: "Не монтировка",
  originalPrice: 4500,
  remainingPieces: 25,
  remainingLimit: 50,
  price: 150,
  orderPieces: 0,
}

export default {
  name: "BusinessManagmentTab",
  components: {BusinessOrderModal, DropDownList, Button},
  props: {
    business: Object
  },
  data: function () {
    return {
      items: [
        testItem1,
        testItem2,
      ],
      extraCharge: {
        min: 75,
        max: 150,
        value: 100,
        inputValue: 100,
      },
      searchValue: "",
      typeFilterOptions: [
        'Показать все',
        'В наличии',
        'Мало товара',
        'Отсутствуют'
      ],
      typeFilterSelectedOption: 'Показать все',
      deliveryPrice: 5000,
      lowAmountThreshold: 0.2,
      highAmountThreshold: 0.8,
      isOrderModalActive: false,
    }
  },
  computed: {
    filteredItems: function () {
      return this.items.filter(item => {
        if (!item.name.toLowerCase().includes(this.searchValue.toLowerCase()))
          return false;

        if (this.typeFilterSelectedOption == this.typeFilterOptions[1]) {
          if (item.remainingPieces <= 0)
            return false;
        } else if (this.typeFilterSelectedOption == this.typeFilterOptions[2]) {
          if (item.remainingPieces >= item.remainingLimit * this.lowAmountThreshold)
            return false;
        } else if (this.typeFilterSelectedOption == this.typeFilterOptions[3]) {
          if (item.remainingLimit > 0)
            return false;
        }
        console.log('kek')
        return true;
      })
    },
    orderedItemsPrice: function () {
      return this.items.reduce((acc, item) => {
        return acc + item.orderPieces * item.originalPrice;
      }, 0)
    }
  },
  methods: {
    incrementItemOrderPieces(item) {
      if (item.orderPieces == item.remainingLimit)
        return;

      item.orderPieces++;
    },
    decrementItemOrderPieces(item) {
      if (item.orderPieces == 0)
        return;

      item.orderPieces--;
    },
    fillItemOrderPieces(item) {
      item.orderPieces = item.remainingLimit - item.remainingPieces;
    },
    fillWarehouse() {
      for (let item of this.items) {
        if (item.remainingPieces >= item.remainingLimit)
          continue;

        item.orderPieces = item.remainingLimit - item.remainingPieces;
      }
    },
    clearOrder() {
      for (let item of this.items) {
        item.orderPieces = 0;
      }
    },
    setExtraChargeInputToMax() {
      this.extraCharge.inputValue = this.extraCharge.max;
    },
    setExtraChargeInputToMin() {
      this.extraCharge.inputValue = this.extraCharge.min;
    },
    applyExtraCharge() {
      this.extraCharge.value = this.extraCharge.inputValue;
    },

    onOrderButtonClick() {
      this.isOrderModalActive = true;
    }
  }
}
</script>

<style lang="scss" scoped>
.business-warehouse-table {
  display: flex;
  flex-direction: column;
  gap: .25vw;

  overflow: hidden;

  position: relative;

  &::after {
    content: "";

    position: absolute;
    bottom: 0;
    left: 0;
    right: .5vw;

    height: 1vw;

    background: linear-gradient(180deg, rgba(19, 32, 44, 0) 0%, #13202C 100%);
  }

  &__items {
    display: flex;
    flex-direction: column;
    gap: .25vw;
    overflow: auto;

    padding-right: .5vw;
    padding-bottom: .5vw;

    position: relative;

    &::-webkit-scrollbar {
      -webkit-appearance: none;

      width: .5vw;
    }

    &::-webkit-scrollbar-track {
      background: rgba(55, 176, 255, 0.1);
      border-radius: 100vw;
    }

    &::-webkit-scrollbar-thumb {
      background: #37B0FF;
      border: .1vw solid rgba(255, 255, 255, 0.2);
      box-shadow: 0px 0px .5vw rgba(0, 164, 255, 0.4), 0px 0px 1.5vw rgba(0, 164, 255, 0.2);
      border-radius: 100vw;
    }
  }

  &__cell {
    flex-shrink: 0;
    flex-grow: 0;

    width: 9vw;

    color: white;

    font-size: .7vw;
    font-weight: 400;
    text-align: left;

    &_column {
      &_original-price {
        flex-grow: 1;
      }

      &_buttons {
        width: fit-content;
      }
    }
  }

  &__row {
    display: flex;
    align-items: center;

    width: 100%;

    box-sizing: border-box;
    padding: .25vw .5vw;

    &_type {
      &_header {
        margin-bottom: .3vw;
      }

      &_item {
        border: .1vw solid rgba(255, 255, 255, 0.1);
        border-radius: .3vw;
        box-shadow: inset 0px .25vw .3vw rgba(0, 0, 0, 0.15);
      }
    }

    transition: background-color 100ms ease-out, border-color 100ms ease-out;

    &_active {
      background: rgba(55, 176, 255, 0.1);
      border-color: #0A9CFB;
    }
  }

  &__buttons {
    display: flex;
    align-items: center;
    gap: .5vw;
  }

  &__amount-button {
    display: flex;
    flex-shrink: 0;
    align-items: center;
    justify-content: center;

    width: 1vw;
    height: 1vw;

    border: .1vw solid #D2D2D6;
    border-radius: .3vw;
  }

  &__amount-max-button {
    height: 1.25vw;
  }

  &__amount-input {
    color: white;

    font-size: .75vw;
    text-align: center;

    min-width: 0;
    width: 1.5vw;

    background: none;
    border: none;
    outline: none;

    border-bottom: .1vw solid rgba(255, 255, 255, 0.1);

    &::-webkit-outer-spin-button, &::-webkit-inner-spin-button {
      -webkit-appearance: none;
    }
  }

  &__value {
    width: fit-content;

    padding: .25vw .4vw;

    background-color: #193246;
    border-radius: .3vw;

    &_theme {
      &_red {
        background-color: #3C1F2D;
      }

      &_green {
        background-color: #02CE0A;
      }

      &_dark-green {
        background-color: rgba(2, 206, 10, 0.1);
      }
    }
  }
}

.business-search-input {
  display: flex;
  align-items: center;
  gap: .25vw;

  width: fit-content;

  padding: .3vw;

  background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);
  border: .1vw solid #FFFFFF;
  border-radius: .3vw;
  box-shadow: inset 0px .25vw .3vw rgba(0, 0, 0, 0.15);

  &__icon {
    height: .8vw;
  }

  &__input {
    color: white;

    font-size: .7vw;

    border: none;
    outline: none;
    background: none;

    &::placeholder {
      color: #8B8D8F;
    }
  }
}

.business-warehouse-label {
  display: flex;
  align-items: center;
  gap: .25vw;

  color: white;

  font-size: .9vw;
  font-weight: 600;

  &__icon {
    height: 1.2vw;
  }
}

.business-warehouse-management {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: .75vw;

  padding: 1.25vw;

  background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);
  border: .1vw solid rgba(255, 255, 255, 0.1);
  box-shadow: 0px 0px 1.5vw rgba(0, 164, 255, 0.05);
  border-radius: 1vw;

  overflow: hidden;

  &__label {

  }

  &__top {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__additional-buttons {
    display: flex;

    padding-bottom: .5vw;
    margin-bottom: .5vw;

    border-bottom: .1vw solid #656B6F;
  }

  &__additional-button-container {
    &:not(:last-child) {
      padding-right: 1vw;
      margin-right: 1vw;
      border-right: .1vw solid #656B6F;
    }
  }

  &__additional-button-label {
    margin-bottom: .25vw;

    color: #D2D2D6;

    font-size: .7vw;
  }

  &__additional-button {
    gap: .4vw;

    font-size: .75vw;
    font-weight: 500;
  }

  &__additional-button-icon {
    height: 1vw;
  }

  &__calculations-container {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  &__calculations-section {
    display: flex;
    align-items: center;
    gap: .25vw;

  }

  &__calculation {
    display: flex;
    align-items: center;
    gap: .25vw;

    height: 2vw;

    color: white;

    font-size: .65vw;
    font-weight: 700;

    &:not(:last-child) {
      padding-right: 1vw;
      margin-right: 1vw;
      border-right: .1vw solid #656B6F;
    }
  }

  &__calculation-icon {
    height: .8vw;
  }

  &__total-container {
    display: flex;
    align-items: center;
    gap: 1.25vw;
  }

  &__total-text {
    color: #0A9CFB;

    font-size: .75vw;
    font-weight: 600;
  }

  &__order-button {
    padding: .6vw 1vw;

    background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);
    border: .1vw solid rgba(255, 255, 255, 0.1);

    &_active {
      background: radial-gradient(50% 50% at 50% 50%, #37B0FF 0%, #0A9CFB 100%);
      border: .1vw solid rgba(255, 255, 255, 0.2);
    }
  }
}


.extra-charge-input {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: .75vw;

  height: fit-content;

  padding: .1vw;
  padding-left: .75vw;

  background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);
  border: .1vw solid #FFFFFF;
  border-radius: .4vw;
  box-shadow: inset 0px .25vw .3vw rgba(0, 0, 0, 0.15);

  &__input {
    width: 14vw;

    color: white;

    font-size: .7vw;

    background: none;
    border: none;
    outline: none;

    &::-webkit-inner-spin-button, &::-webkit-outer-spin-button {
      -webkit-appearance: none;
    }
  }

  &__buttons {
    display: flex;
    align-items: center;
    gap: .5vw;
  }

  &__range-button {
    height: 1vw;
  }

  &__save-button {
    padding: .2vw .4vw;

    color: #8B8D8F;

    font-size: .7vw;

    background: rgba(55, 176, 255, 0.1);
    border: .1vw solid rgba(255, 255, 255, 0.05);
    border-radius: .3vw;
  }
}

.business-extra-charge-edit {
  display: flex;
  justify-content: space-between;
  align-items: center;

  padding: .5vw .75vw;

  background: linear-gradient(180deg, rgba(20, 27, 35, 0.98) 0%, rgba(20, 34, 47, 0.98) 100%);
  border: .1vw solid rgba(255, 255, 255, 0.1);
  box-shadow: 0px 0px 1.5vw rgba(0, 164, 255, 0.05);
  border-radius: 1vw;

  &__brief-container {
    display: flex;
    gap: .25vw;
  }

  &__brief-icon {
    height: 1vw;

    padding-top: .25vw;
  }

  &__brief-title {
    color: white;

    font-size: 1.25vw;
    font-weight: 700;
  }

  &__brief-subtitle {
    color: #0A9CFB;

    font-size: .7vw;
  }
}

.business-management-tab {
  display: flex;
  flex-direction: column;
  gap: 1vw;


  overflow: hidden;
  //height: 70vh;
}
</style>