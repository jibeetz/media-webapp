<template>
    <div>

        <section v-if="suggestedPublications.length">
            <h2>Suggested podcasts</h2>
            <ul>
                <li v-for="(suggestedPublication, key) in suggestedPublications" :key="key">
                    <button v-on:click="$emit('suggestPodcast', suggestedPublication)">{{suggestedPublication.title}}</button>
                </li>
            </ul>

        </section>

    </div>
</template>

<script>
export default {
  name: "suggested-publications",
  data() {
    return {
      suggestedPublications: [
        {
          title: "atp",
          url: "atp.fm/episodes?format=rss"
        },
        {
          title: "Rogan Local",
          url: "http://aweso.me/experiments/feed.xml"
        },
        {
          title: "Jordan Peterson",
          url: "https://rss.art19.com/the-jordan-b-peterson-podcast"
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
          url: "https://www.youtube.com/feeds/videos.xml?channel_id=UCzQUP1qoWDoEbmsQxvdjxgQ"
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
