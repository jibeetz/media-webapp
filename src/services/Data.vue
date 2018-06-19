<script>

import axios from 'axios';

export default {
  name: 'data',
  getPodcast(podcastUrl) {
    return axios.get('https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D"' + podcastUrl + '"&format=json')
  }
}
</script>