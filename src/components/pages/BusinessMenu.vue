<template>
  <div class="business-menu">
    <BusinessModal class="business-menu__modal" theme="menu">
      <template #header>
        <div class="business-menu__header">
          <p class="business-menu__business-name">
            {{ business.name }}
          </p>
          <div class="business-menu__show-chat">
            <Checkbox v-model="showChat"/>
            Показать чат
          </div>
        </div>
      </template>

      <TabSwitch :options="tabs" v-model="currentTab"/>
      <BusinessInformationTab v-if="currentTab === tabs[0]" :business="business"/>
      <BusinessStatsTab v-if="currentTab === tabs[1]" :business="business"/>
      <BusinessManagmentTab v-if="currentTab === tabs[2]" :business="business"/>
    </BusinessModal>
  </div>
</template>

<script>
import alertIcon from '@/assets/business/img/icons/alert-circle.svg';
import growIcon from '@/assets/business/img/icons/trending-up.svg';
import settingsIcon from '@/assets/business/img/icons/settings.svg';

import BusinessModal from "../organisms/BusinessModal";
import TabSwitch from "../organisms/TabSwitch";
import BusinessInformationTab from "./menu-tabs/BusinessInformationTab";
import BusinessManagmentTab from "./menu-tabs/BusinessManagmentTab";
import Checkbox from "../atoms/Checkbox";
import BusinessStatsTab from "./menu-tabs/BusinessStatsTab";

const tabs = [
  {
    title: "Информация о бизнесе",
    icon: alertIcon
  },
  {
    title: "Статистика",
    icon: growIcon
  },
  {
    title: "Управление",
    icon: settingsIcon
  },
];

export default {
  name: "BusinessMenu",
  components: {BusinessStatsTab, Checkbox, BusinessManagmentTab, BusinessInformationTab, TabSwitch, BusinessModal},
  data: function () {
    return {
      tabs,
      currentTab: tabs[0],
      showChat: false,
      business: {
        image: "https://s3-alpha-sig.figma.com/img/c1f7/2f97/72c627325877236b16d94782f5ef3363?Expires=1652054400&Signature=WXGdWxWJQntc5Hkh4ksa~sFN~nx6TMzVlHCwY6p1O8p50T343Bgm0298Ktf1z95er0XxeZLP7tAl0iILRLvaiAyezE3JU1ne9jcDhH68jR3a~K1j5HCjCw7Y-6pDMHp-NjYO0TBx6bf4eKb~nna4fYMq~xSyhJEXwuH-fVIu6QreRLuCvhknIJfFEupwMy~5xwupcm94zKzw49a7o9ZM-XVRhhwrHtmC0l~xP5rBGZd5PMMCTvMQaVjkt~AU3DbaReMu0DKsXclmQ6~5~jEA4FrSGv4tWWEZJ-fMIPUJ5PVTHnD-S2HP4q4BTYkuoSwUskqih-3xVdk2SDQHNY01qQ__&Key-Pair-Id=APKAINTVSUGEWH5XD5UA",
        name: "Магазин 24/7",
        owner: "Violet Rodentino",
        type: "Магазин 24/7",
        statePrice: "55 000 000$",
        rentalPrice: "5 000$",
        fees: "15%",
        criminalRoof: "Армянская мафия",
        balance: "250 000 000$",
        items: [
          {
            name: "Монтировка",
            remainingPieces: 25,
            price: 150,
            originalPrice: 4500,
          }
        ]
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.business-menu {
  display: flex;
  align-items: center;
  justify-content: center;

  height: 100vh;

  &__modal {
    display: flex;
    flex-direction: column;
    gap: 1vw;

    width: 52vw;
    height: 84vh;

    box-sizing: border-box;
    padding: 2vw 1.5vw;

  }

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__show-chat {
    display: flex;
    align-items: center;
    gap: .25vw;

    font-size: .75vw;
  }
}
</style>