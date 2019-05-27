<template>
    <div>

        <h2>Player</h2>

        <button v-if="loadedEpisode" v-on:click="removeLoadedEpisode(loadedEpisode)">Remove episode</button>

        <h3 v-if="loadedEpisode">{{loadedEpisode.title}}</h3>
        {{playerView}}
        <div v-if="loadedEpisode && playerView === 'yt'">
            <div id="yt-player"></div>
        </div>

        <div v-if="loadedEpisode && playerView === 'text'">
            {{loadedEpisode.description}}
        </div>

        <div v-if="loadedEpisode && playerView === 'sound'">
            <audio controls v-bind:src="loadedEpisode.enclosure.url" id="audiotrack" preload="auto" itemprop="audio"></audio>
        </div>

    </div>
</template>

<script>
export default {
  name: "player",
  data() {
    return {
      loadedEpisode: null,
      playerView: null
    };
  },

  props: ["playedEpisode"],
  watch: {
    playedEpisode: function(playedEpisode) {
      if(playedEpisode)
        this.playEpisode(playedEpisode.episode);
    }
  },

  methods: {
    playEpisode: function(episode) {
      this.setViewType(episode);

      if (!this.loadedEpisode ||(this.loadedEpisode && this.loadedEpisode.guid !== episode.guid)) {

        this.loadedEpisode = episode;
        localStorage.setItem('loadedEpisode', JSON.stringify(this.loadedEpisode));

        return;
      }

      if (this.loadedEpisode && this.loadedEpisode.guid === episode.guid) {
        console.log("already loaded", episode);
      }

    },
    setViewType: function(episode) {

      if (episode.id && episode.id.substring(0, 2) === "yt") {
        this.playerView = "yt";

        this.$nextTick(function() {
          this.loadYoutubeVideo(episode);
        });

        return;
      }

      if (!episode.enclosure || !episode.enclosure.url) {
        this.playerView = "text";
        return;
      }

      if (episode.enclosure && episode.enclosure.url) {
        this.playerView = "sound";
        return;
      }
    },
    removeLoadedEpisode: function(loadedEpisode) {
      this.loadedEpisode = null;
      localStorage.setItem("loadedEpisode", null);
      this.playerView = null;
      this.$emit('removeLoadedEpisode', true)
    },
    setupYoutube: function() {
      let tag = document.createElement("script");
      tag.src = "https://www.youtube.com/iframe_api";
      let firstScriptTag = document.getElementsByTagName("script")[0];
      firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
    },
    loadYoutubeVideo: function(episode) {
      let player;

      player = new YT.Player("yt-player", {
        height: "360",
        width: "640",
        videoId: episode.videoId,
        events: {
          onReady: onPlayerReady,
          onStateChange: onPlayerStateChange
        }
      });

      function onPlayerReady(event) {
        // event.target.playVideo();
      }

      function onPlayerStateChange() {}
    }
  },
  mounted: function() {
    this.loadedEpisode = localStorage.getItem("loadedEpisode") ? JSON.parse(localStorage.getItem("loadedEpisode")) : null;
    console.log("loadedEpisode", this.loadedEpisode);

    this.setupYoutube();
  }
};
</script>