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

        <PodcastAdd @addPodcast="addPodcast" :open="isAddOpen"/>

      </header>

      <div class="left-pane">

        <PodcastsList @addEpisode="addEpisode" :added="addedPodcast" @playEpisode="playEpisode"/>

        <Playlists :addedEpisode="addedEpisode" @playEpisode="playEpisode"/>

      </div>

      <div class="right-pane">

        <Player :playedEpisode="playedEpisode"/>

      </div>



    </div>
</template>

<script>

import PodcastsList from '@/components/PodcastsList'
import PodcastAdd from '@/components/PodcastAdd'
import Playlists from '@/components/Playlists'
import Player from '@/components/Player'

export default {
  name: 'Podcast',
  components: {
    PodcastsList,
    PodcastAdd,
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
      console.log('e', );
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
  display: grid;
  height: 100vh;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
  "header header"
  "left-pane left-pane"
  "right-pane right-pane";

  @media screen and (min-width: 769px) and(orientation: landscape){

    grid-template-areas:
    "header header"
    "left-pane right-pane"
  }
}

.header {
  grid-area: header;

  &.dede{
    color: red
  }
}

.left-pane {
  grid-area: left-pane;
}

.right-pane {
  grid-area: right-pane;
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
