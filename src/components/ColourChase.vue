<template>
  <div>
      <h1>Colour Chase</h1>
      <div class="colours">
          <div v-for="colour in colours" v-bind:key="colour.name">
              <svg
                class="colour"
                v-on:click="toggleColour(colour)"
                v-bind:class="[colour.name, { dead: colour.dead }]"
                width="36mm"
                height="46.44mm"
                version="1.1"
                viewBox="0 0 36 46.44"
                xmlns="http://www.w3.org/2000/svg">
                <defs>
                <filter id="a" x="0" y="0" width="1" height="1.29" color-interpolation-filters="sRGB">
                <feFlood flood-color="rgb(0,0,0)" flood-opacity=".77" result="flood"/>
                <feComposite in="flood" in2="SourceGraphic" operator="out" result="composite1"/>
                <feGaussianBlur in="composite1" result="blur" stdDeviation="4.0700000000000003"/>
                <feOffset dx="0" dy="-5" result="offset"/>
                <feComposite in="offset" in2="SourceGraphic" operator="atop" result="composite2"/>
                </filter>
                </defs>
                <g transform="translate(0 -250.6)">
                <circle cx="18" cy="268.6" r="18" filter="url(#a)"/>
                <path d="m19.36 259.9c-0.0945-3.331-0.2835-5.363-3.307-7.772 1.195-0.1762 2.188-0.15 3.134-0.0765 2.061 1.276 2.669 3.976 2.701 7.872z" fill="#fff"/>
                <path d="m14.78 260.6c-0.8504-2.551-3.331-5.386-4.512-6.307 0.8789-0.6147 1.703-1.009 2.52-1.376 1.99 1.488 3.354 3.22 4.308 6.904-0.6116 0.3056-1.269 0.5981-2.315 0.7796z" fill="#fff"/>
                </g>
              </svg>
          </div>
      </div>

      <div class="players">
          <p class="add" v-on:click="addPlayer()">+</p>
          <div v-for="player in players" v-bind:key="player.id">
            <p
              class="remove"
              v-on:click="players.splice(players.indexOf(player), 1)">×</p>

            <input v-model="player.name" />

            <div
              v-if="player.colour"
              class="pcolour"
              v-bind:class="[player.colour.name, { 'hide-colour': !player.visible }]"
              v-on:click="toggleVisible(player)"></div>

            <p class="get" v-else v-on:click="getColour(player)">?</p>

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
    addPlayer () {
      var id = Math.max.apply(Math, this.players.map(player => player.id)) + 1

      if (id < 0) id = 1

      this.players.push({ id: id, name: 'player' + id })
    },

    toggleColour (colour) {
      colour.dead = !colour.dead

      var p = this.players.filter(player => player.colour === colour)

      for (let player of p) {
        player.colour = null
      }
    },

    getColour (player) {
      var undead = this.colours.filter(colour => !colour.dead)
      var unclaimed = undead.filter(
        colour => !this.players.some(player => player.colour === colour)
      )

      if (unclaimed.length > 0) {
        var colour = unclaimed[Math.floor(Math.random() * unclaimed.length)]
        Vue.set(player, 'colour', colour)
        Vue.set(player, 'visible', true)
      }
    },

    toggleVisible (player) {
      Vue.set(player, 'visible', !player.visible)
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

.pcolour {
  display: inline-block;
  width: 20px;
  height: 20px;
}

.yellow {
  background-color: yellow;
  fill: yellow;
}

.green {
  fill: green;
  background-color: green;
}

.brown {
  fill: brown;
  background-color: brown;
}

.blue {
  fill: blue;
  background-color: blue;
}

.pink {
  fill: pink;
  background-color: pink;
}

.black {
  fill: black;
  background-color: black;
}

.hide-colour {
    background-color: grey;
}

.colour {
  width: 100px;
  height: 100px;
  background-color: transparent;
  line-height: 100px;
  clip-path: circle();
  vertical-align: middle;
}

.dead {
  opacity: 0.3;
}

.add, .remove, .get {
    display: inline-block;
    height: 40px;
    padding: 5px;
    margin: 5px;
    font-size: 2rem;
    font-weight: bolder;
    vertical-align: middle;
}

input {
    height: 40px;
    font-size: 1.5rem;
    border: none;
    padding: 5px 10px;
    margin: 5px;
    line-height: 20px;
    width: 150px;
}

input:focus {
    outline: none;
    border-top: none;
    border-left: none;
    border-right: none;
    border-bottom: 3px solid blue;
}
</style>
