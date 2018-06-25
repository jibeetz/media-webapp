<template>
    <div>

        <section v-if="suggestedPodcasts.length">
            <h2>Suggested podcasts</h2>
            <ul>
                <li v-for="(suggestedPodcast, key) in suggestedPodcasts" :key="key">
                    <button v-on:click="$emit('suggestPodcast', suggestedPodcast)">{{suggestedPodcast.title}}</button>
                </li>
            </ul>

        </section>

    </div>
</template>

<script>
export default {
  name: "podcast-suggested",
  data() {
    return {
      suggestedPodcasts: [
        {
          title: "atp",
          url: "atp.fm/episodes?format=rss"
        },
        {
          title: "Joe Rogan",
          url: "http://podcasts.joerogan.net/feed"
        },
        {
          title: "L'after",
          url: "https://podcast.rmc.fr/channel59/RMCInfochannel59.xml"
        },
        {
          title: "PowerfulJRE",
          url:
            "https://www.youtube.com/feeds/videos.xml?channel_id=UCzQUP1qoWDoEbmsQxvdjxgQ"
        }
      ]
    };
  }
};
</script>

<style scoped lang="scss">
ul {
  display: flex;
  padding: 0;

  li {
    list-style: none
  }
}
</style>
