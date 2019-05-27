<template>
    <div v-bind:class="{ 'list-open': selectedPodcast !== null }">

        <h2>podcasts list</h2>

        <ul v-if="savedPodcasts.length">
          <li v-for="(savedPodcast, key) in savedPodcasts" :key="key">
            {{savedPodcast.title}}
            <button v-on:click="removePodcast(savedPodcast)">Remove</button>
            <button v-on:click="toggleEpisodes(savedPodcast)">Toggle episodes</button>
            <button v-on:click="updatePodcast(savedPodcast)">Update podcast</button>

            <ul class="episodes-list" v-show="listedEpisodes[convertToSlug(savedPodcast.title)]">
              <li v-for="(episode, key) in savedPodcast.item" :key="key">
                {{episode.title}}
                <button v-on:click="$emit('addEpisode', {episode})">Add episode</button>
                <button v-on:click="$emit('playEpisode', {episode})">Play episode</button>
              </li>
            </ul>

            <!-- <ul v-if="selectedPodcast !== null && selectedPodcast === savedPodcast" class="episodes-list">
              <li v-for="(episode, key) in selectedPodcast.item" :key="key">
                {{episode.title}}
                <button v-on:click="$emit('addEpisode', {episode})">Add episode</button>
                <button v-on:click="$emit('playEpisode', {episode})">Play episode</button>
              </li>
            </ul> -->

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
      updatedPodcast: null,
      listedEpisodes: {}
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
        console.log('closeAddPanel', );
        this.$emit('closeAddPanel', true)

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
    toggleEpisodes: function (savedPodcast) {
      let channelSlug = this.convertToSlug(savedPodcast.title);
      let isDiplayed = (this.listedEpisodes[channelSlug]) ? false : true;
      this.$set(this.listedEpisodes, channelSlug, isDiplayed);
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
    },
    convertToSlug(text){
      return text
        .toLowerCase()
        .replace(/[^\w ]+/g,'')
        .replace(/ +/g,'-')
        ;
    }
  },
  mounted: function () {

    this.savedPodcasts = (localStorage.getItem('podcastsList')) ? JSON.parse(localStorage.getItem('podcastsList')) : [];

  }
}
</script>


<style scoped lang="scss">

.list-open {

}

</style>
