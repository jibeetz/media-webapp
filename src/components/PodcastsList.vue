<template>
    <div>
        <h2>podcasts list</h2>

        <ul v-if="savedPodcasts.length">
          <li v-for="(savedPodcast, key) in savedPodcasts" :key="key">
            {{savedPodcast}}
          </li>
        </ul>
    </div>
</template>

<script>
export default {
  name: 'podcasts-list',
  data () {
    return {
      savedPodcasts: []
    }
  },

  props: ['added'],
  watch: {
    added: function(addedPodcast) {
      console.log('addedPodcast ', addedPodcast)

      this.savePodcast(addedPodcast)
    }
  },

  methods: {
    savePodcast: function(addedPodcast){

      if(this.savedPodcasts.indexOf(addedPodcast.link) != -1){

          console.log('already saved');

          // setTimeout(() => {
          //     this.message = '';
          // }, 3000);
          return;
        }

        this.savedPodcasts.push(addedPodcast.link);

        localStorage.setItem('podcastsList', JSON.stringify(this.savedPodcasts));

        // this.$emit('removePodcast');
    }
  },
  mounted: function () {

    let savedPodcasts = localStorage.getItem('podcastsList');

    console.log('savedePodcasts', savedPodcasts);

    savedPodcasts = (localStorage.getItem('podcastsList')) ? JSON.parse(savedPodcasts) : [];

    this.savedPodcasts = savedPodcasts
  }
}
</script>


<style scoped>

</style>
