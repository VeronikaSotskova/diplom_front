<template lang="pug">
CirclePacking
</template>

<script>

import CirclePacking from "@/components/CirclePacking";

export default {
  name: 'App',
  components: {
    CirclePacking
  }
}
</script>

<style lang="scss">
@font-face {
  font-style: normal;
  font-weight: 400;
  font-family: "Montserrat";
  font-display: swap;
  src:
      url("assets/fonts/montserrat-regular.woff2") format("woff2"),
      url("assets/fonts/montserrat-regular.woff") format("woff");
}


@font-face {
  font-style: normal;
  font-weight: 500;
  font-family: "Montserrat";
  font-display: swap;
  src:
      url("assets/fonts/montserrat-medium.woff2") format("woff2"),
      url("assets/fonts/montserrat-medium.woff") format("woff");
}

@font-face {
  font-style: normal;
  font-weight: 600;
  font-family: "Montserrat";
  font-display: swap;
  src:
      url("assets/fonts/montserrat-semibold.woff2") format("woff2"),
      url("assets/fonts/montserrat-semibold.woff") format("woff");
}

@font-face {
  font-style: normal;
  font-weight: 700;
  font-family: "Montserrat";
  font-display: swap;
  src:
      url("assets/fonts/montserrat-bold.woff2") format("woff2"),
      url("assets/fonts/montserrat-bold.woff") format("woff");
}

#app {
  font-family: "Montserrat", "Arial", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
