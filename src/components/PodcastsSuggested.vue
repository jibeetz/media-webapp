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
    name: 'podcast-suggested',
    data () {
        return {
            suggestedPodcasts: [
                {
                title: 'atp',
                url: 'atp.fm/episodes?format=rss'
                },
                {
                    title: 'L\'after',
                    url: 'https://rmcsport.bfmtv.com/rss/emission/after-foot/'
                }
            ]
        }
    }
}
</script>