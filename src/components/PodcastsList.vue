<template>
    <div>
        <h2>podcasts list</h2>

        <ul v-if="savedPodcasts.length">
          <li v-for="(savedPodcast, key) in savedPodcasts" :key="key">
            {{savedPodcast.title}}
            <button v-on:click="removePodcast(savedPodcast)">Remove</button>
            <button v-on:click="showEpisodes(savedPodcast)">Show episodes</button>
            <button v-on:click="updatePodcast(savedPodcast)">Update podcast</button>
          </li>
        </ul>

        <button v-if="selectedPodcast !== null" v-on:click="hideEpisodes()">Hide episodes</button>
        <ul v-if="selectedPodcast !== null">
          <li v-for="(episode, key) in selectedPodcast.item" :key="key">
            {{episode.title}}
            <button v-on:click="$emit('addEpisode', {episode})">Add episode</button>
            <button v-on:click="$emit('playEpisode', {episode})">Play episode</button>
          </li>
        </ul>
    </div>
</template>

<script>

import Data from '../services/Data'
import Normalize from '../services/Normalize'

export default {
  name: 'podcasts-list',
  data () {
    return {
      savedPodcasts: [],
      selectedPodcast: null,
      updatedPodcast: null
    }
  },

  props: ['added'],
  watch: {
    added: function(addedPodcast) {
      console.log('watch added', addedPodcast);
      this.savePodcast(addedPodcast)
    }
  },

  methods: {
    savePodcast: function(addedPodcast){

      let checkIfAlreadySaved = this.savedPodcasts.filter(savedPodcast => savedPodcast.url === addedPodcast.loadedPodcast.url)

      if(checkIfAlreadySaved.length !== 0){

          console.log('already saved');

          // setTimeout(() => {
          //     this.message = '';
          // }, 3000);
          return;
        }

        this.savedPodcasts.push(addedPodcast.loadedPodcast);

        localStorage.setItem('podcastsList', JSON.stringify(this.savedPodcasts));

        // this.$emit('removePodcast');
    },
    removePodcast: function(podcast){

      var index = this.savedPodcasts.indexOf(podcast);
      if (index > -1) {
        this.savedPodcasts.splice(index, 1);

        localStorage.setItem('podcastsList', JSON.stringify(this.savedPodcasts));

        if(this.selectedPodcast === podcast){
          this.selectedPodcast = null
        }
      }

    },
    showEpisodes: function (podcast){
      // console.log('showEpisodes', podcast);
      this.selectedPodcast = podcast;
    },
    hideEpisodes: function(){
      this.selectedPodcast = null;
    },
    updatePodcast: function(podcast){

      Data.getPodcast(podcast.url).then(response => {

        this.updatedPodcast = Normalize.set(response.data, this.podcastUrl);

        this.comparePodcasts(podcast, this.updatedPodcast);
      })

    },
    comparePodcasts: function(currentPodcast, newPodcast){

      console.log('currentPodcast', currentPodcast);
      console.log('newPodcast', newPodcast);

      let currentPodcastBuildDate = new Date(currentPodcast.lastBuildDate)
      let newPodcastBuildDate = new Date(newPodcast.lastBuildDate)

      console.log('currentPodcastBuildDate', currentPodcastBuildDate);
      console.log('newPodcastBuildDate', newPodcastBuildDate);

      if(currentPodcastBuildDate.getTime() == newPodcastBuildDate.getTime()){
        console.log('same', );
      }

      if(currentPodcastBuildDate < newPodcastBuildDate){
        console.log('diff', );

        currentPodcast.lastBuildDate = newPodcast.lastBuildDate

        // let podcastToUpdateIndex = this.savedPodcasts.indexOf(currentPodcast)

        // this.savedPodcasts.splice(podcastToUpdateIndex, 1, newPodcast);

        // localStorage.setItem('podcastsList', JSON.stringify(this.savedPodcasts));

        // if(this.selectedPodcast === currentPodcast){
        //   this.selectedPodcast = newPodcast
        // }

      }
    }
  },
  mounted: function () {

    this.savedPodcasts = (localStorage.getItem('podcastsList')) ? JSON.parse(localStorage.getItem('podcastsList')) : [];

  }
}
</script>


<style scoped>

</style>
