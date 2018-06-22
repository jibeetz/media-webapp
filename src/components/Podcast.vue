<template>
    <div>
        <div>{{message}}</div>

        <h1>{{title}}</h1>
        isAddOpen: {{isAddOpen}}
        <button v-on:click="isAddOpen = !isAddOpen">
          <span v-if="!isAddOpen">Show</span>
          <span v-if="isAddOpen">Hide</span>
          List
        </button>

        <PodcastAdd @addPodcast="addPodcast" :open="isAddOpen"/>

        <PodcastsList @addEpisode="addEpisode" :added="addedPodcast" @playEpisode="playEpisode"/>

        <Playlists :addedEpisode="addedEpisode" @playEpisode="playEpisode"/>

        <Player :playedEpisode="playedEpisode"/>

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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
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
