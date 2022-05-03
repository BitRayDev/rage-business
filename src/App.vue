<template>
  <div id="app">
<!--    <BusinessBrief/>-->
    <BusinessMenu/>
  </div>
</template>

<script>
import BusinessBrief from "./components/pages/BusinessBrief";
import BusinessMenu from "./components/pages/BusinessMenu";
export default {
  name: 'App',
  components: {BusinessMenu, BusinessBrief},
}
</script>

<style lang="scss">
@import "assets/business/fonts/roboto/stylesheet.css";

* {
  font-family: 'Roboto';

  margin: 0;
  padding: 0;

  user-select: none;
  -webkit-user-drag: none;
}
</style>
