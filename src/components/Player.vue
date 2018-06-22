<template>
    <div>

        <h2>Player</h2>

        <button v-if="loadedEpisode" v-on:click="removeLoadedEpisode(loadedEpisode)">Remove episode</button>

        <h3 v-if="loadedEpisode">{{loadedEpisode.title}}</h3>
        {{playerView}}
        <div v-if="loadedEpisode && playerView === 'yt'">
            {{loadedEpisode.link.href}}
            <iframe width="560" height="315" v-bind:src="loadedEpisode.link.href" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

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
    name: 'player',
    data () {
        return {
            loadedEpisode: null,
            playerView: null
        }
    },

    props: ['playedEpisode'],
    watch: {
        playedEpisode: function(playedEpisode) {
            this.playEpisode(playedEpisode.episode)
        }
    },

    methods: {
        playEpisode: function(episode){

            this.setViewType(episode);

            if(!this.loadedEpisode){

                this.loadedEpisode = episode;
                localStorage.setItem('loadedEpisode', JSON.stringify(this.loadedEpisode));

            }else{
                if(this.loadedEpisode.guid === episode.guid){
                    console.log('already loaded', episode);
                }

                if(this.loadedEpisode.guid !== episode.guid){
                    this.loadedEpisode = episode;
                    localStorage.setItem('loadedEpisode', JSON.stringify(this.loadedEpisode));
                }
            }
        },
        setViewType: function(episode){
            console.log('episode', episode);
            if(episode.id && episode.id.substring(0, 2) === 'yt'){
                this.playerView = 'yt'
                return;
            }

            if(!episode.enclosure || !episode.enclosure.url){
                this.playerView = 'text'
                return;
            }

            if(episode.enclosure && episode.enclosure.url){
                this.playerView = 'sound'
                return;
            }
        },
        removeLoadedEpisode: function(loadedEpisode){
            this.loadedEpisode = null
            localStorage.setItem('loadedEpisode', null);
            this.playerView = null;
        }
    },
  mounted: function () {

    this.loadedEpisode = (localStorage.getItem('loadedEpisode')) ? JSON.parse(localStorage.getItem('loadedEpisode')) : null;
    console.log('loadedEpisode', this.loadedEpisode);
  }
}
</script>