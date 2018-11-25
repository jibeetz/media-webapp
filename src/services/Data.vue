<script>

import axios from 'axios';

export default {
  name: 'data',
  getPodcast(podcastUrl) {
    return axios.get('http://aweso.me/experiments/?dst=' + podcastUrl)
  }
}
</script>