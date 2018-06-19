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

        <ul v-if="selectedPodcast !== null">
          <li v-for="(episode, key) in selectedPodcast.item" :key="key">
            {{episode.title}}
          </li>
        </ul>
    </div>
</template>

<script>

import Data from '../services/Data'

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

      console.log('addedPodcast', addedPodcast);

      if(this.savedPodcasts.indexOf(addedPodcast.loadedPodcast) != -1){

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
      }

    },
    showEpisodes: function (podcast){
      console.log('showEpisodes', podcast);
      this.selectedPodcast = podcast;
    },
    updatePodcast: function(podcast){

      Data.getPodcast(podcast.url).then(response => {

        this.updatedPodcast = response.data.query.results.rss.channel

        this.comparePodcasts(podcast, this.updatedPodcast);
      })

    },
    comparePodcasts: function(currentPodcast, newPodcast){

      let currentPodcastBuildDate = new Date(currentPodcast.lastBuildDate)
      let newPodcastBuildDate = new Date(newPodcast.lastBuildDate)

      console.log('currentPodcastBuildDate', currentPodcastBuildDate);
      console.log('newPodcastBuildDate', newPodcastBuildDate);

      if(+currentPodcastBuildDate == +newPodcastBuildDate){
        console.log('same', );
      }

      if(currentPodcastBuildDate < newPodcastBuildDate){
        console.log('diff', );

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
