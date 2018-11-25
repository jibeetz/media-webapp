<script>

export default {
  name: 'organize-data',
  set(data, podcastUrl) {

    let podcast = {}

    if(data.entry){
      podcast = data
      podcast.type = 'yt'
      podcast.lastBuildDate = data.published
      podcast.item = data.entry
    }

    if(data.channel){
      podcast = data.channel
      podcast.type = 'default'
    }

    podcast.url = podcastUrl

    return podcast;

  }
}
</script>