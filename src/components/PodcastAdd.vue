<template>
    <div>

      <PodcastsSuggested @suggestPodcast="suggestPodcast"/>

      <input type="text" v-model="podcastUrl">

      <button v-on:click="getPodcast">Add podcast</button>

      <section v-if="loadedPodcast">
          <h4>{{loadedPodcast.title}}</h4>
          <button v-if="podcastUrl" v-on:click="$emit('savePodcast', loadedPodcast)">Save</button>
          <button v-if="podcastUrl" v-on:click="removeLoadedPodcast">Remove</button>

          <ul>
              <li v-for="episode in loadedPodcast.item" :key="episode.guid.content">
                  {{episode.title}}
              </li>
          </ul>
      </section>
    </div>
</template>

<script>

import PodcastsSuggested from '@/components/PodcastsSuggested'

export default {
  name: 'podcast-add',
  components: {
    PodcastsSuggested
	},
  data () {
    return {
      podcastUrl: 'atp.fm/episodes?format=rss',
      loadedPodcast: null
    }
  },
  watch: {

  },
  methods: {
    getPodcast () {
      console.log('podcastUrl', this.podcastUrl);

      if(this.podcastUrl === ''){
        console.log('blank');
        return;
      }

      this.$http.get('https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D"' + this.podcastUrl + '"&format=json').then(response => {
        this.loadedPodcast = response.body.query.results.rss.channel
        console.log('loadedPodcast', this.loadedPodcast);

      }, response => {
        // error callback
      })
    },
    removeLoadedPodcast(){
      this.loadedPodcast = {}
      this.podcastUrl = ''
    },
    suggestPodcast(suggestedPodcast){
      console.log('suggestedPodcast', suggestedPodcast);

      this.podcastUrl = suggestedPodcast.url

      this.getPodcast()

    }
  }
}
</script>

<style scoped>
</style>
