<template>
  <div class="" id="app">
    <div class="background"></div>
    <div class="logo">
      <img alt="StockSphere logo" :src="require('@/assets/images/logo_stocksphere.png')">
    </div>
      <StockPrice />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import StockPrice from '@/components/StockPrice.vue';

export default defineComponent({
  name: 'App',
  components: {
    StockPrice,
  },
});
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden; /* Prevents scrolling */
  height: 100%; /* Full height */
  width: 100%; /* Full width */
}
.background {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('assets/images/background2.png');
  /*background-color: black;*/
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  z-index: -1; 
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  height: 100dvh;
  box-sizing: border-box;
  /* border: 2px solid #00ccfff3; */

}

.logo img {
  /* border: 5px solid #00ffc8f3; */
  width: 370px;
  margin-top: 40px
}
.logo {
  display: flex;
  justify-content: center;
  /* border: 5px solid #ff006af3; */
}


</style>
