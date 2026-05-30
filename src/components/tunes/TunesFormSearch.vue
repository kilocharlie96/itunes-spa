<template>
  <form action="#" @submit.prevent="getMusic()">
    <input
      class="input"
      v-model="query"
      type="text"
      autofocus
      placeholder="Zadaj názov piesne, alebo interpreta"
    />
  </form>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      query: '',
      limit: 5,
    }
  },
  methods: {
    getMusic() {
      axios
        .get(
          `https://itunes.apple.com/search?term=${encodeURIComponent(this.query)}&entity=song&limit=${this.limit}`,
        )
        .then((response) => {
          let iTunesSongs = response.data.results
            .filter((song) => song.kind === 'song')
            .map((song) => this.extractData(song))

          this.$emit('add-new-songs', iTunesSongs)
        })
        .catch((error) => {
          console.error('Chyba pri načítaní dát:', error)
        })

      // axios.get(`https://itunes.apple.com/search?term=${encodeURIComponent(this.query)}&entity=song&limit=${this.limit}`)
      //     .then(response => {
      //         response.data.results.forEach(song => {
      //             this.$emit('add-new-song', this.extractData(song))
      //         });
      //     })
      //     .catch(error => {
      //         console.error("Chyba pri načítaní dát:", error);
      //     });
    },
    extractData({
      trackId: id,
      artistName: artist,
      previewUrl: audioFile,
      artworkUrl100: cover,
      trackName: name,
      collectionName: album,
    }) {
      return { id, artist, audioFile, cover, name, album }
    },
  },
}
</script>
