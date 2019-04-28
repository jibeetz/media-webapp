<template>
    <div class="app-container">

      <header class="header">
        <div>{{message}}</div>

        <h1>{{title}}</h1>
        <button v-on:click="isAddOpen = !isAddOpen">
           Add
          <span v-if="!isAddOpen">+</span>
          <span v-if="isAddOpen">-</span>

        </button>

        <AddPublication @addPodcast="addPodcast" :open="isAddOpen"/>

      </header>

      <div class="left-pane">

        <Publications @addEpisode="addEpisode" :added="addedPodcast" @playEpisode="playEpisode"/>

        <Playlists :addedEpisode="addedEpisode" @playEpisode="playEpisode"/>

      </div>

      <div class="right-pane">
        <Player :playedEpisode="playedEpisode"/>
      </div>
    </div>
</template>

<script>

import Publications from '@/components/Publications/List'
import AddPublication from '@/components/Publications/Add'
import Playlists from '@/components/Playlists'
import Player from '@/components/Player'

export default {
  name: 'manager',
  components: {
    Publications,
    AddPublication,
    Playlists,
    Player
	},
  data () {
    return {
      title: 'Media app',
      message: null,
      addedPodcast: null,
      addedEpisode: null,
      playedEpisode: null,
      isAddOpen: false
    }
  },
  methods: {
    addPodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
    },
    addEpisode: function(addedEpisode){
      this.addedEpisode = addedEpisode
    },
    playEpisode: function(playedEpisode){
      this.playedEpisode = playedEpisode
    }
  }
}
</script>

<style scoped lang="scss">

.app-container{

}

.header {

  &.dede{
    color: red
  }
}

.left-pane {
}

.right-pane {
}

h1,
h2 {
  font-weight: normal;
  margin: 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
