<template>
  <div id="app">
    <h1>Demons Killed: {{demons}}</h1>
    <h2>Fist level: {{fists}}</h2>
    <button v-if="!disabled" v-on:click="killFirstDemon">Kill your first demon on accident.</button><br>
    <button v-on:click="upgradeFists">Upgrade fists ({{fistCost}})</button>
  </div>
</template>

<script>
  export default {
    name: "app",
    data(){
      return{
        //first kill button hider
        disabled: false,

        //currency amounts and upgrades
        demons: 0,
        fists: 0,

        //costs and multipliers
        fistCost: 1,
        fistCostMultiplier: 1.5,
      }
    },
    methods: {
      killFirstDemon: function () {
        this.demons++;
        this.disabled = true;
      },

      killDemons: function (amount) {
        this.demons = this.demons += amount;
      },
      upgradeFists: function () {
        if (this.demons >= this.fistCost){
          this.fists++;
          this.demons = this.demons - this.fistCost;
          this.fistCost = this.fistCost * this.fistCostMultiplier;
        }
      }
    },
    mounted() {
      let that = this;
      setInterval(function () {
        if (that.fists >= 1) {
          that.killDemons(that.fists);
        }


        //title
        const title = "Demons killed: " + that.demons;
        document.title = title;
      }, 1000)
    },
  }
</script>

<style>
  body{
    text-align: center;
    background-image: url("https://66.media.tumblr.com/1c5b251b8f2962e87e0b7b0384b6df3d/tumblr_o89aziL6FE1qd4q8ao1_400.gifv");
  }
</style>