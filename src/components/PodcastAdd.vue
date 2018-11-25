<template>

    <div>

      <div class="podcast_add" :style="{maxHeight: podcastHeight  + 'px'}">

      <div class="podcast_add-container">
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
    </div>
    </div>
</template>

<script>

import PodcastsSuggested from '@/components/PodcastsSuggested'
import Data from '../services/Data'
import Normalize from '../services/Normalize'

export default {
  name: 'podcast-add',
  components: {
    PodcastsSuggested
	},
  data () {
    return {
      podcastUrl: 'atp.fm/episodes?format=rss',
      loadedPodcast: null,
      podcastHeight: 0,
      podcastContainerHeight: 0
    }
  },
  props: ['open'],
  watch: {
    open: {
      immediate: true,
      handler(isAddOpen) {
        this.updateState(isAddOpen);
      },
    }
  },
  methods: {
    updateState(isAddOpen){

      if(document.getElementsByClassName('podcast_add-container')[0])
        this.podcastContainerHeight = document.getElementsByClassName('podcast_add-container')[0].clientHeight;

      this.podcastHeight = (isAddOpen) ? this.podcastContainerHeight : 0;

    },
    loadPodcast () {

      if(this.podcastUrl === ''){
        console.log('blank');
        return;
      }

      Data.getPodcast(this.podcastUrl).then(response => {

        this.loadedPodcast = Normalize.set(response.data, this.podcastUrl);

        console.log('this.loadedPodcast', this.loadedPodcast);

        this.$nextTick(function() {
          this.podcastHeight = document.getElementsByClassName('podcast_add-container')[0].clientHeight;
        });

      }, response => {
        // error callback
      })
    },

    unloadPodcast(){
      this.loadedPodcast = {}
      this.podcastUrl = ''
      this.$nextTick(function() {
        this.podcastHeight = document.getElementsByClassName('podcast_add-container')[0].clientHeight;
      });
    },
    suggestPodcast(suggestedPodcast){
      console.log('suggestedPeeodcast', suggestedPodcast);

      this.podcastUrl = suggestedPodcast.url

      this.loadPodcast()

    }
  },
  computed: {

  },
  mounted: function() {

    this.podcastContainerHeight = document.getElementsByClassName('podcast_add-container')[0].clientHeight;

  }
}
</script>

<style scoped lang="scss">

.podcast_add{
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.5s;
}

</style>
