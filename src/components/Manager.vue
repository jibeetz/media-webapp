<template>
    <div class="app-container" v-bind:class="{ dark: isDarkMode }">

      <AddPublication @addPodcast="addPodcast" @closeAddPanel="onCloseAddPanel" :open="isAddOpen"/>

      <Settings @toggleSettingsPanel="onToggleSettingsPanel" :open="isSettingsOpen" @toggleDarkMode="onToggleDarkMode"></Settings>

      <header class="header">
        <div>{{message}}</div>

        <button class="btn trigger-menu" v-on:click="isSettingsOpen = !isSettingsOpen">
          <span v-if="!isSettingsOpen">Open</span>
          <span v-if="isSettingsOpen">Close</span>
          settings
        </button>

        <h1>{{title}}</h1>
        <button class="btn-add" v-on:click="isAddOpen = !isAddOpen">
           Add
          <span v-if="!isAddOpen">+</span>
          <span v-if="isAddOpen">-</span>
        </button>

      </header>

      <div class="panes">

        <div class="pane list-pane">

          <Publications @addEpisode="addEpisode" :added="addedPodcast" @playEpisode="playEpisode" @closeAddPanel="onCloseAddPanel"/>

          <Playlists :addedEpisode="addedEpisode" @playEpisode="playEpisode"/>

        </div>

        <div class="pane player-pane" v-bind:class="{ open: playedEpisode !== null }">
          <Player :playedEpisode="playedEpisode"/>
        </div>

      </div>
    </div>
</template>

<script>

import Publications from '@/components/Publications/List'
import AddPublication from '@/components/Publications/Add'
import Playlists from '@/components/Playlists'
import Player from '@/components/Player'
import Settings from '@/components/Settings'

export default {
  name: 'manager',
  components: {
    Settings,
    Publications,
    AddPublication,
    Playlists,
    Player
	},
  data () {
    return {
      title: 'RSS app',
      message: null,
      addedPodcast: null,
      addedEpisode: null,
      playedEpisode: null,
      isAddOpen: false,
      isSettingsOpen: false,
      isDarkMode: null
    }
  },
  methods: {
    addPodcast: function (addedPodcast) {
      this.addedPodcast = addedPodcast;
    },
    addEpisode: function(addedEpisode){
      this.addedEpisode = addedEpisode
    },
    playEpisode: function(playedEpisode){
      this.playedEpisode = playedEpisode
    },
    onCloseAddPanel (value) {
      this.isAddOpen = !value
    },
    onToggleSettingsPanel (value) {
      this.isSettingsOpen = !value
    },
    onToggleDarkMode (value) {
      this.isDarkMode = value;
    }
  }
}
</script>

<style scoped lang="scss">

.app-container{
  display: flex;
  flex-direction: column;
  height: 100vh;
  color: #2c3e50;
  transition: background-color 0.3s, color 0.3s;

  &.dark {
    color: #fff;
    background-color: #2c3e50;
  }
}

.header {
  padding: 15px;
}

.panes {
  display: flex;
  height: calc(100vh - 30px);
}

.pane {
  flex: 0 0 calc(50% - 30px);
  padding: 15px;
}

.list-pane {
  @media screen  and (max-width: 768px)  {
    flex: 0 0 calc(100% - 30px);
  }
}

.player-pane {
  @media screen  and (max-width: 768px)  {
    position: fixed;
    background-color: red;
    bottom: 0;
    width: 100%;
    height: 50px;
  }
}

.btn-add {

}

h1,
h2 {
  font-weight: normal;
  margin: 0;
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
