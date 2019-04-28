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

import Publication from '@/services/Publication'

export default {
  name: 'publications-list',
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

      Publication.get(podcast.url).then(response => {

        this.updatedPodcast = Publication.standardize(response.data, this.podcastUrl);

        this.compareShows(podcast, this.updatedPodcast);
      })

    },
    compareShows: function(currentPodcast, newPodcast){

      console.log('currentPodcast', currentPodcast);
      console.log('newPodcast', newPodcast);

      let currentPodcastLastShowDate = new Date(currentPodcast.item[0].pubDate)
      let newPodcastLastShowDate = new Date(newPodcast.item[0].pubDate)

      console.log('currentPodcastLastShowDate', currentPodcastLastShowDate);
      console.log('newPodcastLastShowDate', newPodcastLastShowDate);

      if(currentPodcastLastShowDate.getTime() == newPodcastLastShowDate.getTime()){
        console.log('same', );
      }

      if(currentPodcastLastShowDate < newPodcastLastShowDate){
        console.log('diff', );

        // currentPodcast.lastBuildDate = newPodcast.lastBuildDate

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
