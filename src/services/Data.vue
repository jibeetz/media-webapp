export default {
  getPodcast(podcastUrl) {
    console.log('podcastUrl', podcastUrl);
    return this.$http.get('https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20xml%20where%20url%3D"' + podcastUrl + '"&format=json')
  }
}