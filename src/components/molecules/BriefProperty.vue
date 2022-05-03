<template>
  <div class="brief-property">
    <p class="brief-property__title">{{title}}</p>
    <p :class="`brief-property__value ${theme ? `brief-property__value_theme_${theme}` : ''}`">
      <img class="brief-property__icon" v-if="icon" :src="icon"/>
      {{value}}
    </p>
  </div>
</template>

<script>
export default {
  name: "BriefProperty",
  props: {
    title: String,
    icon: {
      type: String,
      required: false
    },
    value: String,
    theme: String,
  }
}
</script>

<style lang="scss" scoped>
.brief-property {
  &__title {
    margin-bottom: .2vw;

    color: #D2D2D6;

    font-weight: 700;
    font-size: .6vw;
  }

  &__value {
    display: flex;
    align-items: center;
    gap: .5vw;

    padding: .4vw .7vw;

    color: white;

    font-size: .75vw;

    background: rgba(55, 176, 255, 0.1);
    border-radius: 5px;

    &_theme {
      &_blue-text {
        color: #0A9CFB;
      }
      &_green {
        background-color: #143025;
        color: #02CE0A;
      }
    }
  }

  &__icon {
    height: .8vw;
  }
}
</style>