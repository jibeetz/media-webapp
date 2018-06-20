<template>
    <div>
        <h2>Playlists</h2>

    </div>
</template>

<script>

export default {
  name: 'Playlists',
  components: {
	},
  data () {
    return {
    }
  },
  methods: {
    addPodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
    }
  }
}
</script>

<style scoped>

</style>
