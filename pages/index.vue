<template>
<div id="app">
  <div id="wrapper">
    <p>My horrible Game :')</p>
    <div class="w1">
      <div class="b1">
        <div v-if="!gameIsRunning">
          <button @click="startGame" id="start-game">Start</button>
        </div>

        <div v-else>
          <div>
            <button id="attack" @click="attack">Attack</button>
            <button id="special-attack" @click="specialAttack">S Attack</button>
          </div>
          <div>
            <button id="heal" @click="heal">Heal</button>
            <button id="give-up" @click="giveUp">Retreat</button>
          </div>
        </div>
      </div>

      <div class="r1" v-if="turns.length > 0">
        <ul>
          <li v-for="turn in turns" :class="{ 'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}">
            {{ turn.text }}
          </li>
        </ul>
      </div>
    </div>

    <div class="w2">
      <div id="Bub">
        <div class="health_empty">
          <div class="health" :style="{width: playerHealth + '%'}">
            {{ playerHealth }}
          </div>
        </div>

        <div class="body">
          <div class="belly"/>
          <div class="eye-r"/>
          <div class="eye-l"/>
        </div>
      </div>

      <div id="Procrastination">
        <div class="ehealth_empty">
          <div class="ehealth" :style="{width: monsterHealth + '%'}">
            {{ monsterHealth }}
          </div>
        </div>

        <div>
          <img src="../static/this.png" style="height: 300px; width: auto;"/>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>

  import styles from '../components/css.css';
  import bub from '../components/bub.css';
  import enemy from '../components/enemy.css';

    export default {
        name: "test",
        components:{styles, bub, enemy},

      data: function () {
        return {
          gameIsRunning: false,
          enemyHP: 100,
          playerHP: 100,
          turns: [],

          return: {}
        }
      },
      methods: {
        startGame: function() {
          this.playerHealth = 100;
          this.monsterHealth = 100;
          this.gameIsRunning = true;
        },
        attack: function() {
          var damage = this.calculateDamage(3, 10);
          this.monsterHealth -= damage;
          this.turns.unshift({
            isPlayer: true,
            text: "Player hits Monster for " + damage
          });

          if (this.checkWin()) {
            return;
          }

          this.monsterAttack();
        },
        specialAttack: function() {
          var damage = this.calculateDamage(10, 20);
          this.monsterHealth -= damage;

          this.turns.unshift({
            isPlayer: true,
            text: "Player special attacks Monster for " + damage
          });

          if (this.checkWin()) {
            return;
          }

          this.monsterAttack();
        },
        heal: function() {
          var healing = this.playerHealth;
          if (this.playerHealth <= 90) {
            this.playerHealth += 10;
          } else {
            this.playerHealth = 100;
          }
          healing = this.playerHealth - healing;
          this.turns.unshift({
            isPlayer: true,
            text: "Player heals " + healing + " health points"
          });
          this.monsterAttack();
        },
        giveUp: function() {
          this.gameIsRunning = false;
          this.turns.length = false;
        },
        calculateDamage: function(min, max) {
          var damage =Math.max(Math.floor(Math.random() * max) + 1, min);
          return damage;
        },
        monsterAttack: function() {
          var damage = this.calculateDamage(5, 12);
          this.playerHealth -= damage;
          this.turns.unshift({
            isPlayer: false,
            text: "Monster hits player for " + damage
          });
          this.checkWin();
        },
        checkWin: function() {
          if (this.monsterHealth <= 0) {
            if (confirm("You won! New Game?")) {
              this.startGame();
              this.turns.length = false;
            } else {
              this.gameIsRunning = false;
            }
            return true;
          } else if (this.playerHealth <= 0) {
            if (confirm("You lost! New Game?")) {
              this.startGame();
              this.turns.length = false;
            } else {
              this.gameIsRunning = false;
            }
            return true;
          }
          return false;
        }
      }
    }
</script>

<style>

</style>
