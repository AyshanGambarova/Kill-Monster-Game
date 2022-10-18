<template>
  <div id="app">
    <div class="row">
      <div class="small-6 columns">
        <h1 class="text-center">You</h1>
        <div class="healthbar">
          <div
          :style="{width:player_heal+'%'}"
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
          >{{player_heal}}%</div>
        </div>
      </div>
      <div class="small-6 columns">
        <h1 class="text-center">Monster</h1>
        <div class="healthbar">
          <div
          :style="{width:monster_heal+'%'}"
            class="healthbar text-center"
            style="background-color: green; margin: 0; color: white;"
          >{{monster_heal}}%</div>
        </div>
      </div>
    </div>
    <div class="row controls" v-if="!game_is_on">
      <div class="small-12 columns">
        <button id="start-game" @click="start_game">New game</button>
      </div>
    </div>
    <div class="row controls" v-if="game_is_on">
      <div class="small-12 columns">
        <button id="attack" @click="attack">Attack</button>
        <button id="special-attack" @click="special_attack">
          Special attack
        </button>
        <button id="heal" @click="heal_up">Heal up</button>
        <button id="give-up" @click="give_up">Give up!</button>
      </div>
    </div>
   
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      player_heal: 100,
      monster_heal: 100,
      game_is_on: false
    };
  },
  methods: {
    start_game: function() {
      this.game_is_on = true;
    },
    attack: function() {
      var point = Math.ceil(Math.random() * 15);
      this.monster_heal -= point;
      this.monster_attack();
    },
    special_attack: function() {
      var point = Math.ceil(Math.random() * 25);
      this.monster_heal -= point;
      this.monster_attack();
    },
    heal_up: function() {
      var point = Math.ceil(Math.random() * 20);
      this.player_heal += point;
      this.monster_attack();
    },
    give_up: function() {
      this.player_heal=0;
    },
    monster_attack: function() {
      var point = Math.ceil(Math.random() * 10);
      this.player_heal -= point;
    }
  },
  watch:{
    player_heal:function (value) { 
      if (value<0) {
        this.player_heal=0
       if ( confirm('You lost the game, do you want to start again?')) {
        this.player_heal=100;
        this.monster_heal=100;
       }
      }
      else if (value>=100) {
        this.player_heal=100
      }
     },
      monster_heal:function (value) { 
      if (value<=0) {
        this.monster_heal=0
        if ( confirm('You won the game, do you want to start again?')) {
        this.player_heal=100;
        this.monster_heal=100;
       }
      }
      
     }
  }
};
</script>

<style>
@import "./assets/foundation.min.css";
.text-center {
  text-align: center;
}

.healthbar {
  width: 100%;
  height: 40px;
  background-color: #eee;
  margin: auto;
  transition: width 500ms;
}

.controls,
.log {
  margin-top: 30px;
  text-align: center;
  padding: 10px;
  border: 1px solid #ccc;
  box-shadow: 0px 3px 6px #ccc;
}

.turn {
  margin-top: 20px;
  margin-bottom: 20px;
  font-weight: bold;
  font-size: 22px;
}

.log ul {
  list-style: none;
  font-weight: bold;
  text-transform: uppercase;
}

.log ul li {
  margin: 5px;
}

.log ul .player-turn {
  color: blue;
  background-color: #e4e8ff;
}

.log ul .monster-turn {
  color: red;
  background-color: #ffc0c1;
}

button {
  font-size: 20px;
  background-color: #eee;
  padding: 12px;
  box-shadow: 0 1px 1px black;
  margin: 10px;
}

#start-game {
  background-color: #aaffb0;
}

#start-game:hover {
  background-color: #76ff7e;
}

#attack {
  background-color: #ff7367;
}

#attack:hover {
  background-color: #ff3f43;
}

#special-attack {
  background-color: #ffaf4f;
}

#special-attack:hover {
  background-color: #ff9a2b;
}

#heal {
  background-color: #aaffb0;
}

#heal:hover {
  background-color: #76ff7e;
}

#give-up {
  background-color: #ffffff;
}

#give-up:hover {
  background-color: #c7c7c7;
}
</style>
