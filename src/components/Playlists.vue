<template>
    <div>
        <h2>Playlists</h2>

        <ul>
            <li v-for="(playlist, key) in playlists" :key="key">
                {{playlist.name}}
            </li>
        </ul>

    </div>
</template>

<script>

export default {
  name: 'Playlists',
  components: {
	},
  data () {
    return {
        playlists: [],
        playlistModel: {
            name: 'default'
        }
    }
  },
  methods: {
    addPodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
    }
  },
  mounted: function () {

    if(!localStorage.getItem('playlists'))
        localStorage.setItem('playlists', JSON.stringify([this.playlistModel]));

    this.playlists = JSON.parse(localStorage.getItem('playlists'));

    console.log('this.playlists', this.playlists);

  }
}
</script>

<style scoped>

</style>
