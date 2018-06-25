<template>
    <div>
        <h2>Playlists</h2>

        <ul>
            <li v-for="(playlist, key) in playlists" :key="key">
                <h3>{{playlist.name}}</h3>

                <ul v-if="playlist.episodes.length !== 0">
                    <li v-for="(episode, ekey) in playlist.episodes" :key="ekey">
                        {{episode.title}}
                        <button v-on:click="removeEpisode(playlist, episode)">Remove from playlist</button>
                        <button v-on:click="$emit('playEpisode', {episode})">Play episode</button>
                    </li>
                </ul>

            </li>
        </ul>

    </div>
</template>

<script>
export default {
  name: "Playlists",
  components: {},
  data() {
    return {
      playlists: [],
      playlistModel: {
        name: "Default playlist",
        id: "default",
        episodes: []
      }
    };
  },

  props: ["addedEpisode"],
  watch: {
    addedEpisode: function(addedEpisode) {
      this.saveEpisode(addedEpisode.episode);
    }
  },

  methods: {
    saveEpisode: function(addedEpisode) {
      if (this.playlists.length === 1) {
        let checkIfAlreadySaved = this.playlists[0].episodes.filter(
          savedPodcast => savedPodcast.guid === addedEpisode.guid
        );

        if (checkIfAlreadySaved.length === 0) {
          this.playlists[0].episodes.push(addedEpisode);
        }
      }

      localStorage.setItem("playlists", JSON.stringify(this.playlists));
    },
    removeEpisode: function(playlist, episode) {
      var index = playlist.episodes.indexOf(episode);

      if (index > -1) {
        playlist.episodes.splice(index, 1);

        localStorage.setItem("playlists", JSON.stringify(this.playlists));
      }
    }
  },
  mounted: function() {
    if (!localStorage.getItem("playlists"))
      localStorage.setItem("playlists", JSON.stringify([this.playlistModel]));

    this.playlists = JSON.parse(localStorage.getItem("playlists"));
  }
};
</script>

<style scoped>
</style>
