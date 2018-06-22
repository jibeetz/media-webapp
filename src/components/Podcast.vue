<template>
    <div>
        <div>{{message}}</div>

        <h1>{{title}}</h1>

        <button v-on:click="isAddOpen = !isAddOpen">toggle Add</button>

        <PodcastAdd @addPodcast="addPodcast" :open="isAddOpen"/>

        <PodcastsList @addEpisode="addEpisode" :added="addedPodcast"/>

        <Playlists :addedEpisode="addedEpisode"/>

        <Player/>

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
      isAddOpen: false
    }
  },
  methods: {
    addPodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
    },
    addEpisode: function(addedEpisode){
      this.addedEpisode = addedEpisode
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
