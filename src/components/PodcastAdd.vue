<template>
    <div>

      <PodcastsSuggested @suggestPodcast="suggestPodcast"/>

        <input type="text" v-model="podcastUrl">

        <button v-on:click="getItem">Add podcast</button>

        <section v-if="podcast">
            <h4>{{podcast.title}}</h4>
            <button v-if="podcastUrl" v-on:click="$emit('savePodcast', podcast)">Save</button>
            <button v-if="podcastUrl" v-on:click="removePodcast">Remove</button>

            <ul>
                <li v-for="episode in podcast.item" :key="episode.guid.content">
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
      podcast: null
    }
  },
  props: ['removed'],
  watch: {
    removed: function() {

      console.log('removed')

      this.podcast = {}
      this.podcastUrl = ''
    }
  },
  methods: {
    getItem () {
      console.log('podcastUrl', this.podcastUrl);

      if(this.podcastUrl === ''){
        console.log('blank');
        return;
      }

      this.$http.get('https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D"' + this.podcastUrl + '"&format=json').then(response => {
        this.podcast = response.body.query.results.rss.channel
        console.log('loaded podcast', this.podcast);
      }, response => {
        // error callback
      })
    },
    removePodcast(){
      this.podcast = {}
      this.podcastUrl = ''
    },
    suggestPodcast(suggestedPodcast){
      console.log('suggestedPodcast', suggestedPodcast);

      this.podcastUrl = suggestedPodcast.url

      this.getItem()

    }
  }
}
</script>

<style scoped>
</style>
