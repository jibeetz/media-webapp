<template>
    <div>
        <div>{{message}}</div>

        <h1>{{ title }}</h1>

        <PodcastsList :added="addedPodcast"/>

        <PodcastAdd @savePodcast="savePodcast"/>

    </div>
</template>

<script>

import PodcastsList from '@/components/PodcastsList'
import PodcastAdd from '@/components/PodcastAdd'

export default {
  name: 'Podcast',
  components: {
    PodcastsList,
    PodcastAdd
	},
  data () {
    return {
      title: 'Podcast',
      message: null,
      addedPodcast: null
    }
  },
  methods: {
    savePodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
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
