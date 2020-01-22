<template>
  <div id="app">
    <h1>Cookies: {{cookie}}</h1>
    <h2>Autoclickers: {{autoclicker}}</h2>
    <h2>Grandma's: {{grandma}}</h2>
    <br>
    <button v-on:click="bakeCookie">Bake Cookies</button><br>
    <button v-on:click="buyClicker">Buy Autoclicker ({{autoclickerPrice}} Cookies)</button><br>
    <button v-on:click="buyGrandma">Buy Grandma ({{grandmaPrice}} Cookies)</button><br>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      //main Cookies
      cookie: 0,

      //grandma
      grandma: 0,
      grandmaPrice: 200,

      //autoclicker
      autoclicker: 0,
      autoclickerPrice: 50,
    }
  },
  methods: {
    bakeCookie: function() {
      this.cookie++;
    },
    clickerBake: function (amount) {
      this.cookie  += amount;
    },
    buyClicker: function () {
      if (this.cookie >= this.autoclickerPrice) {
        this.cookie = this.cookie - this.autoclickerPrice;
        this.autoclicker++;
      }
    },
    buyGrandma: function () {
      if (this.cookie >= this.grandmaPrice) {
        this.cookie = this.cookie - this.grandmaPrice;
        this.grandma++;
      }
    },
    grandmaBake: function (amount) {
      this.cookie += 10 * amount;
    },
  },
  mounted() {
    let that = this;
    setInterval(function () {
        if (that.autoclicker >= 1){
          that.clickerBake(that.autoclicker);
        }
        if (that.grandma >= 1){
          that.grandmaBake(that.grandma);
        }
        const title = "Cookies: " + that.cookie;
        document.title = title;
    }, 1000);
  },
}
</script>

<style>
body{
  text-align: center;
}
</style>
