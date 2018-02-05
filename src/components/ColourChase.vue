<template>
  <div>
      <h1>Colour Chase</h1>
      <div class="colours">
          <div v-for="colour in colours" v-bind:key="colour.name">
              <div class="colour" v-bind:class="[colour.name, { dead: colour.dead }]"
              v-on:click="toggleColour(colour)">
              </div>
          </div>
      </div>

      <div class="players">
          <p v-on:click="players.push({id: players.length, name: ''})">Add Player</p>
          <div v-for="player in players" v-bind:key="player.id">
            <p v-on:click="players.splice(players.indexOf(player), 1)">Remove</p>
            <input v-model="player.name" />

            <div v-if="player.colour" class="pcolour" v-bind:class="[player.colour.name]"></div>
            <p v-else v-on:click="getColour(player)">Get</p>

          </div>
      </div>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  name: 'ColourChase',

  data () {
    return {
      colours: [
        { name: 'yellow', dead: false },
        { name: 'green', dead: false },
        { name: 'brown', dead: false },
        { name: 'blue', dead: true },
        { name: 'pink', dead: false },
        { name: 'black', dead: false }
      ],

      players: []
    }
  },

  methods: {
    toggleColour (colour) {
      colour.dead = !colour.dead

      var p = this.players.filter(player => player.colour === colour)

      for (let player of p) {
        player.colour = null
      }
    },

    getColour (player) {
      var undead = this.colours.filter(colour => !colour.dead)
      var unclaimed = undead.filter(colour => !this.players.some(player => player.colour === colour))

      if (unclaimed.length > 0) {
        var colour = unclaimed[Math.floor(Math.random() * unclaimed.length)]
        Vue.set(player, 'colour', colour)
      }
    }
  }
}
</script>

<style scoped>
p {
    display: inline;
}

.colours {
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.colour {
  width: 100px;
  height: 100px;
  line-height: 100px;
  margin: 10px;
  clip-path: circle();
  vertical-align: middle;
}

.colour.dead::after {
    content: "/";
    display: block;
    font-size: 200px;
    text-align: center;
    color: red;
}

.pcolour {
    display: inline-block;
    width: 20px;
    height: 20px;
}

.yellow {
  background-color: yellow;
}

.green {
  background-color: green;
}

.brown {
  background-color: brown;
}

.blue {
  background-color: blue;
}

.pink {
  background-color: pink;
}

.black {
  background-color: black;
}
</style>
