<template>
  <div id="app">
    <h1>Demons Killed: {{demons}}</h1>

    <ul v-if="pickedUpWeapons.length > 0">
      <h2>Weapons picked up</h2>
      <li v-for="weapon in pickedUpWeapons" v-bind:key="weapon">
        <h2>{{weapon.name}} (Level: {{getWeaponLevel(weapon)}})</h2>
        <button v-if="!selectedWeapon || !selectedWeapon.id === weapon.id" v-on:click="selectWeapon(weapon)">select weapon</button>
      </li>
    </ul>

    <ul v-if="weaponsFound.length > 0">
      <h2>Weapons found</h2>
      <li v-for="weapon in weaponsFound" v-bind:key="weapon">
        <h2>{{weapon.name}}</h2>
        <button>pick up weapon</button>
      </li>
    </ul>

    <div v-if="selectedWeapon">
      <h2>Selected weapon</h2>
      {{selectedWeapon.name}} (DPS: {{getKillLevel(selectedWeapon)}}) <br>
      <button v-on:click="upgradeWeapon">Upgrade weapon. ({{this.selectedWeapon.upgradeCost}} demons.).</button>
    </div>

    <button v-if="needFirstKill" v-on:click="killFirstDemon">Kill your first demon on accident.</button><br>
    <button v-if="demons >= 100 || pistolFound" v-on:click="pickUpWeapon('pistol')">Find pistol.</button>

  </div>
</template>

<script>
  export default {
    name: "app",
    data(){
      return{
        demons: 0,
        //first kill button hider
        needFirstKill: true,
        pistolFound: true,
        // pistolFound: false,
        // pistolUpgrade: false,
        // demonKillsNeededForPistol : 50,

        weapons: [
                {
                  id: 'fist',
                  name: 'Fist',
                  demonsKilled: 1,
                  killLevel: 1,
                  level: 1,
                  multiplier: 2,
                  upgradeCost: 10,
                  upgradeMultiplier: 2
                },
                {
                  id: 'pistol',
                  name: 'Pistol',
                  demonsKilled: 100,
                  killLevel: 3,
                  level: 1,
                  multiplier: 4,
                  upgradeCost: 200,
                  upgradeMultiplier: 5
                }
        ],

        weaponsFound: [],
        pickedUpWeapons: [],
        selectedWeapon: null,
      }
    },
    methods: {
      killFirstDemon: function () {
        this.demons =1;
        this.needFirstKill = false;
        var weapon = this.pickUpWeapon('fist');
        this.selectWeapon(weapon);
        this.start();
      },
      upgradeWeapon: function () {
        if (this.demons >= this.selectedWeapon.upgradeCost) {
          this.demons = this.demons - this.selectedWeapon.upgradeCost;
          this.selectedWeapon.level++;
          this.selectedWeapon.upgradeCost = this.selectedWeapon.upgradeCost * this.selectedWeapon.upgradeMultiplier;
        }
      },

      pickUpWeapon: function (id) {
        for (var i=0; i < this.weapons.length; i++) {
          var weapon = this.weapons[i];
          if (id === weapon.id) {
            this.pickedUpWeapons.push(weapon);
            return weapon;
          }
        }
      },

      selectWeapon: function (weapon) {
        this.selectedWeapon = weapon;
      },

      getKillLevel: function (weapon) {
        return weapon.killLevel = (weapon.level * weapon.multiplier);
      },
      getWeaponLevel: function (weapon) {
        return weapon.level;
      },

      start: function() {
        let that = this;
        setInterval(function () {
          var killLevel = that.getKillLevel(that.selectedWeapon);
          that.killDemons(killLevel);

          //title
          const title = "Demons killed: " + that.demons;
          document.title = title;
        }, 1000);
      },

      // foundPistol: function () {
      //   if (this.demons >=  this.demonKillsNeededForPistol) {
      //     this.pistols++;
      //     this.pistolFound = false;
      //     this.pistolUpgrade = true;
      //   }
      // },

      killDemons: function (amount) {
        this.demons = this.demons += amount;
      },
      // upgradeFists: function () {
      //   if (this.demons >= this.fistCost){
      //     this.fists++;
      //     this.demons = this.demons - this.fistCost;
      //     this.fistCost = this.fistCost * this.fistCostMultiplier;
      //   }
      // },
      // upgradePistol: function () {
      //   if (this.demons >= this.pistolCost) {
      //     this.pistols++;
      //     this.demons = this.demons - this.pistolCost;
      //     this.pistolCost = this.pistolCost * this.pistolCostMultiplier;
      //   }
      // },
    },
  }
</script>

<style>
  body{
    text-align: center;
    /*background-image: url("https://66.media.tumblr.com/1c5b251b8f2962e87e0b7b0384b6df3d/tumblr_o89aziL6FE1qd4q8ao1_400.gifv");*/
  }
</style>