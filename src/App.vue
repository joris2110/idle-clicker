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
        disabled: false,
        demons: 0,
        fists: 0,
        fistCost: 1,
        fistCostMultiplier: 2,
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

<style scoped>
  body{
    text-align: center;
  }
</style>