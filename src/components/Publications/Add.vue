<template>

  <div v-show="isAddOpen">

    <div class="podcast_add">

    <div class="podcast_add-container">
      <SuggestedPublications @suggestPodcast="suggestPodcast"/>

      <input type="text" v-model="podcastUrl">

      <button v-on:click="loadPodcast">Load podcast</button>

      <section v-if="loadedPodcast">
          <h4>{{loadedPodcast.title}}</h4>
          <button v-if="podcastUrl" v-on:click="$emit('addPodcast', {loadedPodcast})">Add podcast</button>
          <button v-if="podcastUrl" v-on:click="previewEpisodes(loadedPodcast)">
            <span v-if="!previewedEpisodes[convertToSlug(loadedPodcast.title)]">Preview Episodes</span>
            <span v-if="previewedEpisodes[convertToSlug(loadedPodcast.title)]">Hide</span>
          </button>
          <button v-if="podcastUrl" v-on:click="unloadPodcast">Cancel</button>

          <ul v-if="podcastUrl && loadedPodcast && previewedEpisodes[convertToSlug(loadedPodcast.title)]">
            <li v-for="(episode, epdkey) in loadedPodcast.item" :key="epdkey">{{episode.title}}</li>
          </ul>

      </section>
    </div>
  </div>
  </div>
</template>

<script>

import SuggestedPublications from '@/components/Publications/Suggested'
import Publication from '@/services/Publication'

export default {
  name: 'add-publication',
  components: {
    SuggestedPublications
	},
  data () {
    return {
      podcastUrl: 'atp.fm/episodes?format=rss',
      loadedPodcast: null,
      previewedEpisodes: {},
      isAddOpen: false
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
      this.isAddOpen = isAddOpen;
    },
    loadPodcast () {

      if(this.podcastUrl === ''){
        console.log('blank');
        return;
      }

      Publication.get(this.podcastUrl).then(response => {

        this.loadedPodcast = Publication.standardize(response.data, this.podcastUrl);

      }, response => {
        // error callback
      })
    },

    unloadPodcast(){
      this.loadedPodcast = null
      this.podcastUrl = ''
    },
    suggestPodcast(suggestedPodcast){

      this.podcastUrl = suggestedPodcast.url

      this.loadPodcast()

    },
    previewEpisodes(episode){
      let episodeSlug = this.convertToSlug(episode.title);
      let isDiplayed = (this.previewedEpisodes[episodeSlug]) ? false : true;
      this.$set(this.previewedEpisodes, episodeSlug, isDiplayed);
    },
    convertToSlug(text){
      return text
        .toLowerCase()
        .replace(/[^\w ]+/g,'')
        .replace(/ +/g,'-')
        ;
    }
  },
  computed: {

  },
  mounted: function() {

  }
}
</script>

<style scoped lang="scss">

.podcast_add{
  transition: max-height 0.5s;
}

</style>
