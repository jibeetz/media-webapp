<script>

export default {
  name: 'organize-data',
  set(response, podcastUrl) {

    let podcast = {}

    if(response.feed){
      podcast = response.feed
      podcast.type = 'yt'
      podcast.lastBuildDate = response.feed.published
      podcast.item = response.feed.entry
    }

    if(response.rss && response.rss.channel){
      podcast = response.rss.channel
      podcast.type = 'default'
    }

    podcast.url = podcastUrl

    return podcast;

  }
}
</script>