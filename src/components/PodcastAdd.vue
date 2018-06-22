<template>
    <div class="podcast-add" :class="{'open': isOpen}">

      <PodcastsSuggested @suggestPodcast="suggestPodcast"/>

      <input type="text" v-model="podcastUrl">

      <button v-on:click="loadPodcast">Load podcast</button>

      <section v-if="loadedPodcast">
          <h4>{{loadedPodcast.title}}</h4>
          <button v-if="podcastUrl" v-on:click="$emit('addPodcast', {loadedPodcast})">Add podcast</button>
          <button v-if="podcastUrl" v-on:click="unloadPodcast">Cancel</button>

          <!-- <ul>
              <li v-for="episode in loadedPodcast.item" :key="episode.guid.content">
                  {{episode.title}}
              </li>
          </ul> -->

      </section>
    </div>
</template>

<script>

import PodcastsSuggested from '@/components/PodcastsSuggested'
import Data from '../services/Data'

export default {
  name: 'podcast-add',
  components: {
    PodcastsSuggested
	},
  data () {
    return {
      podcastUrl: 'atp.fm/episodes?format=rss',
      loadedPodcast: null,
      isOpen: false
    }
  },


  props: ['open'],
  watch: {
    open: function(isAddOpen) {
      console.log('isAddOpen', isAddOpen);
      this.updateState(isAddOpen);
    },
    immediate: true
  },

  methods: {
    updateState(isAddOpen){

      this.isOpen = isAddOpen;
      this.$forceUpdate();
    },
    loadPodcast () {
      console.log('podcastUrl', this.podcastUrl);

      if(this.podcastUrl === ''){
        console.log('blank');
        return;
      }

      Data.getPodcast(this.podcastUrl).then(response => {

        console.log('response', response.data.query.results);

        this.loadedPodcast = {};

        if(response.data.query.results.feed){
          this.loadedPodcast = response.data.query.results.feed
          this.loadedPodcast.type = 'yt'
          this.loadedPodcast.lastBuildDate = response.data.query.results.feed.published
          this.loadedPodcast.item = response.data.query.results.feed.entry
        }

        if(response.data.query.results.rss && response.data.query.results.rss.channel){
          this.loadedPodcast = response.data.query.results.rss.channel
          this.loadedPodcast.type = 'default'
        }

        this.loadedPodcast.url = this.podcastUrl


      }, response => {
        // error callback
      })
    },
    unloadPodcast(){
      this.loadedPodcast = {}
      this.podcastUrl = ''
    },
    suggestPodcast(suggestedPodcast){
      console.log('suggestedPodcast', suggestedPodcast);

      this.podcastUrl = suggestedPodcast.url

      this.loadPodcast()

    }
  },
  computed: {

  },
  mounted: function() {
  }
}
</script>

<style scoped>

.podcast-add{
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.5s;
}

.podcast-add.open{
  max-height: 500px;
}

</style>
