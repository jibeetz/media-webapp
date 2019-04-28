<script>

import axios from 'axios';

export default {
  name: 'publication',
  standardize(data, publicationUrl) {

    let publication = {}

    if(data.entry){
      publication = data
      publication.type = 'yt'
      publication.lastBuildDate = data.published
      publication.item = data.entry
    }

    if(data.channel){
      publication = data.channel
      publication.type = 'default'
    }

    publication.url = publicationUrl

    return publication;

  },
  get(publicationUrl) {
    return axios.get('http://aweso.me/experiments/?dst=' + publicationUrl)
  }
}
</script>