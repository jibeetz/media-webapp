<template>

    <div class="settings" v-bind:class="{ open: isSettingsPanelOpen }">
        Settings {{isSettingsPanelOpen}}

        <button class="" v-on:click="$emit('toggleSettingsPanel', isSettingsPanelOpen)">
          Settings
          <span v-if="!isSettingsPanelOpen">+</span>
          <span v-if="isSettingsPanelOpen">-</span>
        </button>

        <label>
          <button class="" v-on:click="toggleDarkMode()">
            <span v-if="!isDarkMode">Enable</span>
            <span v-if="isDarkMode">Disable</span>
            Dark mode
          </button>
        </label>

        {{isDarkMode}}

  </div>
</template>

<script>

export default {
  name: 'settings',
  data () {
    return {
      isSettingsPanelOpen: false,
      isDarkMode: false
    }
  },
  props: ['open'],
  watch: {
    open: {
      immediate: true,
      handler(isSettingsPanelOpen) {
        this.updateState(isSettingsPanelOpen);
      },
    }
  },
  methods: {
    updateState(isSettingsPanelOpen){
      this.isSettingsPanelOpen = isSettingsPanelOpen;
    },
    toggleDarkMode(){
      this.isDarkMode = !this.isDarkMode;
      localStorage.setItem('darkMode', JSON.stringify(this.isDarkMode));
      this.$emit('toggleDarkMode', this.isDarkMode)
    }
  },
  computed: {

  },
  mounted: function() {
    this.isDarkMode = localStorage.getItem("darkMode") ? JSON.parse(localStorage.getItem("darkMode")) : this.isDarkMode;
    this.$emit('toggleDarkMode', this.isDarkMode)
  }
}
</script>

<style scoped lang="scss">

.settings {
  position: fixed;
  width: calc(50% - 30px);
  height: calc(100vh - 30px);
  padding: 15px;
  transition: transform 0.3s, background-color 0.3s;
  background: #f9f9f9;
  overflow: hidden;
  transform: translateX(200%);
  z-index: 1;

  .dark & {
    background-color: #2c3e50;
  }

  &.open {
    transform: translateX(100%);
  }

  @media screen  and (max-width: 768px)  {
    transform: translateX(100%);
    width: calc(100% - 30px);

    &.open {
      transform: translateX(0%);
    }
  }


}

</style>
